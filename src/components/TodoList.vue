<template>
  <Categories
    :todos="todos"
    @all="() => setCategory('all')"
    @urgent="() => setCategory('urgent')"
    @important="() => setCategory('important')"
    @others="() => setCategory('others')"
    @finished="() => setCategory('finished')"
  />
  <div v-if="showRegisterForm">
    <RegisterTaskVue @close="showRegisterForm = false" @newTodo="reloadTodos" />
  </div>
  <div v-if="showRemoveTask">
    <RemoveTask @close="showRemoveTask = false" @delete="deleteTask" />
  </div>
  <div v-if="showEditTask">
    <EditTask :todoIdToEdit="taskIdToEdit" @close="showEditTask = false" @editForm="reloadTodos" />
  </div>
  <div class="todo_list_container">
    <button class="add_todo" @click="showRegisterForm = true">
      <fa class="icon_plus" :icon="['fas', 'plus']" />
    </button>
    <div class="container_todo_list">
      <section>
        <h1 class="title">Minhas Tarefas</h1>
        <h2>
          Olá <span class="name">Luís Fernando</span>, você tem
          <span class="tasks_pending_span"
            >{{ pendingTasksCount }}
            {{ pendingTasksCount !== 1 ? "tarefas" : "tarefa" }}</span
          >
          pendentes.
        </h2>
        <div class="input_container">
          <input
            class="search_input"
            v-model="searchQuery"
            placeholder="Buscar Tarefas"
            type="text"
          />
          <fa class="icon_search" :icon="['fas', 'magnifying-glass']" />
        </div>
      </section>
      <section class="todos">
        <div
          v-for="(todo, index) in filteredTodos"
          :key="index"
          class="todo_item"
          :class="{ done: todo.done }"
        >
          <div>
            <input class="check_todo" type="checkbox" v-model="todo.done" />
            <h3 :class="{ outlined: todo.done }">{{ todo.content.title }}</h3>
          </div>
          <div class="options_container">
            <span class="tag" :class="todo.category">
              {{ todo.category === "important" ? "Importante" : "Urgente" }}
            </span>
            <fa
              @click="toggleOptions(index)"
              :class="{
                icon_config: true,
                icon_config_active: showOptions[index],
              }"
              :icon="['fas', 'ellipsis-vertical']"
            />
            <div class="options" v-if="showOptions[index]">
              <div
                class="container_select"
                style="
                  display: flex;
                  flex-direction: column;
                  align-items: flex-start;
                "
              >
                <div>
                  <div class="edit_circle"></div>
                  <button @click="handleOpenEditModal(todo.id)">Editar</button>
                </div>
                <div>
                  <div class="remove_circle"></div>
                  <button @click="handleOpenDeleteModal(todo.id)">Excluir</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script setup>
import RegisterTaskVue from "../components/RegisterTask.vue"; // Importa o componente de registro de tarefa
import Categories from "../components/Categories.vue"; // Importa o componente de categorias
import RemoveTask from "../components/RemoveTask.vue"; // Importa o componente de remoção de tarefa
import EditTask from "../components/EditTask.vue"; // Importa o componente de edição de tarefa
import { computed, onMounted, ref, watch } from "vue"; // Importa funções e hooks necessários do Vue

// Referências reativas e variáveis computadas
const showRegisterForm = ref(false); // Controla a exibição do formulário de registro de tarefa
const showRemoveTask = ref(false); // Controla a exibição do modal de remoção de tarefa
const showEditTask = ref(false); // Controla a exibição do modal de edição de tarefa
const showOptions = ref({}); // Armazena o estado de exibição das opções de cada tarefa
const taskIdToRemove = ref(null); // Armazena o ID da tarefa a ser removida
const taskIdToEdit = ref(null); // Armazena o ID da tarefa a ser editada
const searchQuery = ref(""); // Consulta de pesquisa para filtragem de tarefas por título e descrição
const selectedCategory = ref("all"); // Categoria selecionada para filtragem de tarefas
const pendingTasksCount = computed(
  () => todos.value.filter((todo) => !todo.done).length
); // Contagem de tarefas pendentes usando uma variável computada

const todos = ref([]); // Lista de todas as tarefas
const filteredTodos = ref([]); // Lista de tarefas filtradas exibidas

// Observa mudanças na lista de todas as tarefas para salvar no localStorage
watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);

// Ordena as tarefas por prioridade e data de criação ascendente
const todos_asc = computed(() =>
  todos.value.slice().sort((a, b) => {
    const priorityOrder = {
      urgent: 1,
      important: 2,
      others: 3,
    };

    const priorityA = priorityOrder[a.category] || 4;
    const priorityB = priorityOrder[b.category] || 4;

    if (priorityA !== priorityB) {
      return priorityA - priorityB;
    }

    return b.createdAt - a.createdAt;
  })
);

// Manipuladores de eventos e funções relacionadas às tarefas
const handleOpenDeleteModal = (index) => {
  showOptions.value = {}; // Limpa o estado das opções de exibição
  taskIdToRemove.value = index; // Define o ID da tarefa a ser removida
  showRemoveTask.value = true; // Exibe o modal de remoção de tarefa
};

const handleOpenEditModal = (index) => {
  showOptions.value = {}; // Limpa o estado das opções de exibição
  taskIdToEdit.value = index; // Define o ID da tarefa a ser editada
  showEditTask.value = true; // Exibe o modal de edição de tarefa
};

// Recarrega a lista de tarefas do localStorage e atualiza as tarefas filtradas
const reloadTodos = () => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || []; // Carrega as tarefas do localStorage
  updateFilteredTodos(); // Atualiza as tarefas filtradas com base na categoria e na consulta de pesquisa
};

// Alternar a exibição das opções de cada tarefa
const toggleOptions = (index) => {
  showOptions.value = {
    [index]: !showOptions.value[index],
  };
};

// Exclui uma tarefa com base no ID e atualiza a lista de tarefas
const deleteTask = () => {
  showOptions.value = {};
  todos.value = todos.value.filter((todo) => todo.id !== taskIdToRemove.value);
  showRemoveTask.value = false;
  updateFilteredTodos();
};

// Define a categoria selecionada e atualiza as tarefas filtradas
const setCategory = (category) => {
  selectedCategory.value = category; // Define a categoria selecionada
  updateFilteredTodos(); // Atualiza as tarefas filtradas com base na categoria e na consulta de pesquisa
};

// Atualiza as tarefas filtradas com base na categoria e na consulta de pesquisa
const updateFilteredTodos = () => {
  let todosToFilter = todos_asc.value; // Inicia com todas as tarefas ordenadas por prioridade e data

  // Filtra as tarefas com base na categoria selecionada
  if (selectedCategory.value !== "all") {
    todosToFilter = todosToFilter.filter((todo) => {
      if (selectedCategory.value === "finished") {
        return todo.done; // Retorna tarefas finalizadas se a categoria for "finished"
      } else if (selectedCategory.value === "others") {
        return todo.category === null; // Retorna tarefas sem categoria se a categoria for "others"
      }
      return todo.category === selectedCategory.value; // Retorna tarefas da categoria selecionada
    });
  }

  // Filtra as tarefas com base na consulta de pesquisa por título ou descrição
  if (!searchQuery.value.trim()) {
    filteredTodos.value = todosToFilter; // Exibe todas as tarefas se não houver consulta de pesquisa
  } else {
    const search = searchQuery.value.trim().toLowerCase();
    filteredTodos.value = todosToFilter.filter(
      (todo) =>
        todo.content.title.toLowerCase().includes(search) ||
        todo.content.description.toLowerCase().includes(search)
    ); // Filtra tarefas com base na consulta de pesquisa por título ou descrição
  }
};

// Observa mudanças na consulta de pesquisa para atualizar as tarefas filtradas
watch(searchQuery, updateFilteredTodos);

// Carrega as tarefas do localStorage e atualiza as tarefas filtradas ao montar o componente
onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || []; // Carrega as tarefas do localStorage
  updateFilteredTodos(); // Atualiza as tarefas filtradas com base na categoria e na consulta de pesquisa
});
</script>

<style lang="stylus" scoped>
.todo_list_container
  display: flex
  flex-direction: column
  align-items: center
  width: 100%
  position: relative
  background-color: #e9f4fb

.add_todo
  position: absolute
  right: 35px
  bottom: 60px
  background-color: #1ad18f
  border: none
  border-radius: 50%
  cursor: pointer

.icon_plus
  width: 25px
  height: 25px
  padding: 15px
  color: white

.container_todo_list
  width: 70%
  margin-top: 100px

.title
  font-size: 25px
  margin-bottom: 5px
  font-family: 'Gilroy-Bold'

.name
  color: #5caeff
  font-family: 'Gilroy-Bold'

.tasks_pending_span
  color: #5caeff
  text-decoration: underline
  font-family: 'Gilroy-Bold'

.input_container
  position: relative
  display: flex
  align-items: center
  margin-top: 30px
  margin-bottom: 30px

.icon_search
  position: absolute
  right: 15px
  width: 20px
  height: 20px
  opacity: 50%
  color: #c8e0ef

.search_input
  width: 100%
  height: 45px
  box-sizing: border-box
  font-size: 17px
  border: 2px solid #c8e0ef
  opacity: 50%
  border-radius: 5px
  padding: 10px

.todo_item
  display: flex
  justify-content: space-between
  margin-bottom: 20px
  padding: 10px
  border: 1px solid #ccc
  border-radius: 5px
  background-color: #ffffff
  height: 55px
  transition: opacity 0.3s ease

.todo_item.done
  opacity: 0.7

.check_todo
  width: 25px
  height: 25px
  color: #1ad18f
  margin-right: 10px

h3
  font-size: 18px
  margin-bottom: 0
  font-family: 'Gilroy-SemiBold'


.outlined
  text-decoration: line-through
  font-family: 'Gilroy-SemiBold'

.tag
  color: white
  padding: 6px
  border-radius: 10px
  margin-right: 15px

.tag.important
  background: #ffc42e

.tag.urgent
  background: #f491ba

.todo_item div
  display: flex
  justify-content: center
  align-items: center

.icon_config
  margin-right: 10px
  cursor: pointer
  z-index: 1
  color: initial

.icon_config_active
  color: #2693ff

.options_container
  position: relative

.options
  position: absolute
  background-color: #ffffff
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.2)
  padding: 20px
  right: 1px
  top: 10px
  border-radius: 5px

.options button
  all: unset
  cursor: pointer

.edit_circle
  height: 10px
  width: 10px
  background: #1ad18f
  border-radius: 10px
  margin-right: 5px

.remove_circle
  height: 10px
  width: 10px
  background: #d6e6ef
  border-radius: 10px
  margin-right: 5px

.container_select
  gap: 10px
  margin-right: 15px

.todos
  height: 650px
  overflow-y: auto

  &::-webkit-scrollbar
    width: 4px
    background-color: #E9F4FB

  &::-webkit-scrollbar-thumb
    background-color: #888
    border-radius: 4px
</style>
