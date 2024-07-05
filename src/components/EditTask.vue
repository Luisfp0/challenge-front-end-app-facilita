<template>
  <div class="container_form">
    <div class="task_container">
      <fa class="icon_close" :icon="['fas', 'xmark']" @click="closeForm" />
      <h3>Editar tarefa</h3>
      <form @submit.prevent="editTodo">
        <div>
          <h4>Título:</h4>
          <input class="input_text" type="text" v-model="input_title" />
        </div>
        <div>
          <h4>Descrição:</h4>
          <textarea
            class="input_textarea"
            v-model="input_description"
          ></textarea>
        </div>
        <div class="add">
          <div class="options">
            <label>
              <input
                type="radio"
                name="category"
                value="urgent"
                v-model="input_category"
              />
              <div>Urgente</div>
            </label>
            <label>
              <input
                type="radio"
                name="category"
                value="important"
                v-model="input_category"
              />
              <div>Importante</div>
            </label>
          </div>
          <input class="submit" type="submit" value="Editar" />
        </div>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps, ref, getCurrentInstance, onMounted } from "vue";

// Definição das propriedades recebidas pelo componente
const props = defineProps({
  todoIdToEdit: String,
});

// Variáveis reativas para os campos do formulário e para a lista de tarefas
const input_title = ref("");
const input_description = ref("");
const input_category = ref<string | null>(null); // Variável para armazenar a categoria selecionada
const todos = ref([]); // Lista de tarefas
const instance = getCurrentInstance();

// Função executada quando o componente é montado para carregar tarefas salvas no localStorage
onMounted(() => {
  const storedTodos = JSON.parse(localStorage.getItem("todos")) || [];
  todos.value = storedTodos;

  // Encontra a tarefa com o ID específico em storedTodos
  const todoToEdit = storedTodos.find((todo) => todo.id === props.todoIdToEdit);

  // Se encontrar a tarefa, preenche os campos do formulário com suas informações
  if (todoToEdit) {
    input_title.value = todoToEdit.content.title;
    input_description.value = todoToEdit.content.description;
    input_category.value = todoToEdit.category;
  }
});

// Função para editar a tarefa existente
const editTodo = () => {
  // Verifica se os campos obrigatórios estão preenchidos
  if (
    input_title.value.trim() === "" ||
    input_description.value.trim() === ""
  ) {
    alert("Titulo e descrição obrigatórios.");
    return;
  }

  // Encontra a tarefa a ser editada na lista de tarefas pelo ID
  const todoToEdit = todos.value.find((todo) => todo.id === props.todoIdToEdit);

  // Atualiza os dados da tarefa com os novos valores
  todoToEdit.content.title = input_title.value;
  todoToEdit.content.description = input_description.value;
  todoToEdit.category = input_category.value;

  // Salva a lista atualizada no localStorage
  saveTodos();

  // Emite um evento para notificar a edição de uma tarefa
  if (instance) {
    instance.emit("editForm");
  }

  // Fecha o formulário após editar a tarefa
  closeForm();
};

// Função para salvar a lista de tarefas no localStorage
const saveTodos = () => {
  localStorage.setItem("todos", JSON.stringify(todos.value));
};

// Função para fechar o formulário de edição de tarefas
const closeForm = () => {
  // Emite um evento para notificar o fechamento do formulário
  if (instance) {
    instance.emit("close");
  }
};
</script>

<style lang="stylus" scoped>
.container_form
  position: absolute
  z-index: 1
  top: 50%
  left: 50%
  transform: translate(-50%, -50%)
  width: 100%
  height: 100%
  background-color: rgba(0, 0, 0, 0.315)
  display: flex
  justify-content: center
  align-items: center

.task_container
  position: relative
  width: 450px
  background-color: #ffffff
  padding: 30px
  border: 1px solid #ccc
  border-radius: 5px
  margin-top: 10px
  display: flex
  flex-direction: column

.task_container h3
  font-size: 23px
  margin-bottom: 30px
  font-family: 'Gilroy-Bold'

.icon_close
  position: absolute
  right: 25px
  top: 25px
  cursor: pointer

.task_container h4
  font-size: 15px
  margin-bottom: 4px

.task_container form
  display: flex
  flex-direction: column
  gap: 15px
  height: 100%

.input_text,
.input_textarea
  width: 100%
  font-size: 17px
  border: 2px solid rgba(0, 0, 0, 0.363)
  border-radius: 5px
  padding: 10px
  box-sizing: border-box

.input_textarea
  height: 200px

.input_text:focus,
.input_textarea:focus
  border-color: #2693ff
  outline: none

.add
  display: flex
  justify-content: space-between
  align-items: center

.options
  display: flex
  gap: 10px

.options label
  display: flex
  align-items: center

.options input
  margin-right: 5px
  width: 20px
  height: 20px

.submit
  color: white
  font-size: 15px
  width: 100px
  height: 40px
  border: none
  border-radius: 7px
  background: #1ad18f
  cursor: pointer
</style>
