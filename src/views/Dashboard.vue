<template>
  <div class="dashboard">
    <Sidebar />
    <div class="main_content">
      <Header />
      <div class="container_todo">
        <Categories />
        <TodoList />
      </div>
    </div>
  </div>
</template>

<script setup>
import Sidebar from "../components/SideBar.vue";
import Header from "../components/Header.vue";
import Categories from "../components/Categories.vue";
import TodoList from "../components/TodoList.vue";
import RegisterTask from '../components/RegisterTask.vue'
</script>

<style scoped>
.dashboard {
  display: flex;
  height: 100vh;
  position: relative;
}

.main_content {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.container_todo {
  display: flex;
  height: 100%;
  width: 100%;
}
</style>
