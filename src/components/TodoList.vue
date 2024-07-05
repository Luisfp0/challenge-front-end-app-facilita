<template>
  <Categories
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
            >{{ todos.length }}
            {{ todos.length > 1 ? "tarefas" : "tarefa" }}</span
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
                  <button>Editar</button>
                </div>
                <div>
                  <div class="remove_circle"></div>
                  <button @click="handleOpenDeleteModal(index)">Excluir</button>
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
import RegisterTaskVue from "../components/RegisterTask.vue";
import Categories from "../components/Categories.vue";
import RemoveTask from "../components/RemoveTask.vue";
import { computed, onMounted, ref, watch } from "vue";

const showRegisterForm = ref(false);
const showRemoveTask = ref(false);
const showOptions = ref({});
const taskIdToRemove = ref(null);
const searchQuery = ref("");
const selectedCategory = ref("all");

const todos = ref([]);
const filteredTodos = ref([]);

watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

const handleOpenDeleteModal = (index) => {
  showOptions.value = {};
  taskIdToRemove.value = index;
  showRemoveTask.value = true;
};

const reloadTodos = () => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
  updateFilteredTodos();
};

const toggleOptions = (index) => {
  showOptions.value = {
    [index]: !showOptions.value[index],
  };
};

const deleteTask = (taskId) => {
  showOptions.value = {};
  todos.value = todos.value.filter(
    (_, index) => index !== taskIdToRemove.value
  );
  showRemoveTask.value = false;
  updateFilteredTodos();
};

const filterByCategory = (category) => {
  if (category === "all") {
    filteredTodos.value = todos_asc.value;
  } else if (category === "finished") {
    filteredTodos.value = todos_asc.value.filter((todo) => todo.done);
  } else {
    filteredTodos.value = todos_asc.value.filter(
      (todo) => todo.category === category
    );
  }
};

const setCategory = (category) => {
  selectedCategory.value = category;
  updateFilteredTodos();
};

const updateFilteredTodos = () => {
  let todosToFilter = todos_asc.value;

  if (selectedCategory.value !== "all") {
    todosToFilter = todosToFilter.filter((todo) => {
      if (selectedCategory.value === "finished") {
        return todo.done;
      }
      return todo.category === selectedCategory.value;
    });
  }

  if (!searchQuery.value.trim()) {
    filteredTodos.value = todosToFilter;
  } else {
    const search = searchQuery.value.trim().toLowerCase();
    filteredTodos.value = todosToFilter.filter((todo) =>
      todo.content.title.toLowerCase().includes(search)
    );
  }
};

watch(searchQuery, updateFilteredTodos);

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
  updateFilteredTodos();
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
  right: 30px
  bottom: 30px
  background-color: #1ad18f
  border: none
  border-radius: 50%

.icon_plus
  width: 25px
  height: 25px
  padding: 15px
  color: white

.container_todo_list
  width: 70%
  margin-top: 250px

.title
  font-size: 25px
  margin-bottom: 5px

.name
  color: #5caeff

.tasks_pending_span
  color: #5caeff
  text-decoration: underline

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

.outlined
  text-decoration: line-through

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
</style>
