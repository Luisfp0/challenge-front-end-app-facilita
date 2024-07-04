<template>
  <div class="todo_list_container">
    <button class="add_todo">
      <fa class="icon_plus" :icon="['fas', 'plus']" />
    </button>
    <div class="container_todo_list">
      <section class="greeting">
        <h1 class="title">Minhas Tarefas</h1>
        <h2>
          Olá <span class="name">Luís Fernando</span>, você tem
          <span class="tasks_pending_span">4 tarefas</span> pendentes.
        </h2>
      </section>
      <section>
        <div class="input_container">
          <input
            class="search_input"
            placeholder="Buscar Tarefas"
            type="text"
          />
          <fa class="icon_search" :icon="['fas', 'magnifying-glass']" />
        </div>
      </section>
    </div>
  </div>
</template>

<script lang="ts">
// Script TypeScript aqui
</script>

<style scoped>
.todo_list_container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  position: relative;
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
  width: 50%;
}

.greeting {
  margin-bottom: 25px;
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
}

.icon_search {
  position: absolute;
  right: 15px;
  width: 20px;
  height: 20px;
  opacity: 50%;
}

.search_input {
  width: 100%;
  height: 45px;
  box-sizing: border-box;
  font-size: 17px;
  border: 2px solid black;
  opacity: 50%;
  border-radius: 5px;
  padding: 10px;
}
</style>
