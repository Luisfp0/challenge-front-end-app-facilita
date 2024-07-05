<template>
  <div class="container_form">
    <div class="task_container">
      <fa class="icon_close" :icon="['fas', 'xmark']" @click="closeForm" />
      <h3>Cadastrar Tarefa</h3>
      <form @submit.prevent="addTodo">
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
          <input class="submit" type="submit" value="Adicionar" />
        </div>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, getCurrentInstance, onMounted } from "vue";

const input_title = ref("");
const input_description = ref("");
const todos = ref([]);
let input_category = ref<string | null>(null);
const instance = getCurrentInstance();

onMounted(() => {
  const storedTodos = JSON.parse(localStorage.getItem("todos")) || [];
  todos.value = storedTodos;
});

const addTodo = () => {
  if (
    input_title.value.trim() === "" ||
    input_description.value.trim() === "" ||
    input_category.value === null
  ) {
    return;
  }

  todos.value.push({
    content: {
      title: input_title.value,
      description: input_description.value,
    },
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),
  });

  input_title.value = "";
  input_description.value = "";
  input_category.value = null;

  saveTodos();
  if (instance) {
    instance.emit("newTodo");
  }
  closeForm();
};

const saveTodos = () => {
  localStorage.setItem("todos", JSON.stringify(todos.value));
};

const closeForm = () => {
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
