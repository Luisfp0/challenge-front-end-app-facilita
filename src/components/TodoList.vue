<template>
  <div v-if="showRegisterForm">
    <RegisterTaskVue @close="showRegisterForm = false" @newTodo="reloadTodos" />
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
            placeholder="Buscar Tarefas"
            type="text"
          />
          <fa class="icon_search" :icon="['fas', 'magnifying-glass']" />
        </div>
      </section>
      <section class="todos">
        <div
          v-for="(todo, index) in todos_asc"
          :key="index"
          class="todo_item"
          :class="{ done: todo.done }"
        >
          <div>
            <input class="check_todo" type="checkbox" v-model="todo.done" />
            <h3 :class="{ outlined: todo.done }">{{ todo.content.title }}</h3>
          </div>
          <div>
            <span class="tag">
              {{ todo.category === "important" ? "Importante" : "Urgente" }}
            </span>
            <fa class="icon_config" :icon="['fas', 'ellipsis-vertical']" />
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script setup>
import RegisterTaskVue from "../components/RegisterTask.vue";
import { computed, onMounted, ref, watch } from "vue";

const showRegisterForm = ref(false);

const todos = ref([]);

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

const reloadTodos = () => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
};

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>

<style scoped>
.todo_list_container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  position: relative;
  background-color: #e9f4fb;
}

.add_todo {
  position: absolute;
  right: 30px;
  bottom: 30px;
  background-color: #1ad18f;
  border: none;
  border-radius: 50%;
}

.icon_plus {
  width: 25px;
  height: 25px;
  padding: 15px;
  color: white;
}

.container_todo_list {
  width: 70%;
  margin-top: 250px;
}

.title {
  font-size: 25px;
  margin-bottom: 5px;
}

.name {
  color: #5caeff;
}

.tasks_pending_span {
  color: #5caeff;
  text-decoration: underline;
}

.input_container {
  position: relative;
  display: flex;
  align-items: center;
  margin-top: 30px;
  margin-bottom: 30px;
}

.icon_search {
  position: absolute;
  right: 15px;
  width: 20px;
  height: 20px;
  opacity: 50%;
  color: #c8e0ef;
}

.search_input {
  width: 100%;
  height: 45px;
  box-sizing: border-box;
  font-size: 17px;
  border: 2px solid #c8e0ef;
  opacity: 50%;
  border-radius: 5px;
  padding: 10px;
}

.todo_item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #ffffff;
  height: 55px;
  transition: opacity 0.3s ease;
}

.todo_item.done {
  opacity: 0.7;
}

.check_todo {
  width: 25px;
  height: 25px;
  color: #1ad18f;
  margin-right: 10px;
}

.h3 {
  font-size: 18px;
  margin-bottom: 0;
}

.outlined {
  text-decoration: line-through;
}

.tag {
  color: white;
  background: pink;
  padding: 6px;
  border-radius: 10px;
  margin-right: 15px;
}

.todo_item div {
  display: flex;
  justify-content: center;
  align-items: center;
}

.icon_config {
  margin-right: 10px;
  cursor: pointer;
}
</style>
