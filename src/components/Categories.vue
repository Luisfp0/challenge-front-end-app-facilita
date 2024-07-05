<template>
  <div class="categories">
    <h2>Categorias</h2>
    <div class="options">
      <div class="category_option" @click="allFilter">
        <div class="underline">
          <fa class="icon" :icon="['fas', 'chevron-right']" />Todas
        </div>
      </div>
      <div class="category_option" @click="urgentFilter">
        <div class="underline">
          <fa class="icon" :icon="['fas', 'chevron-right']" />Urgentes
        </div>
        <div class="circle_span urgent">
          <span>{{ urgentCount }}</span>
        </div>
      </div>
      <div class="category_option" @click="importantFilter">
        <div class="underline">
          <fa class="icon" :icon="['fas', 'chevron-right']" />Importantes
        </div>
        <div class="circle_span important">
          <span>{{ importantCount }}</span>
        </div>
      </div>
      <div class="category_option" @click="othersFilter">
        <div class="underline">
          <fa class="icon" :icon="['fas', 'chevron-right']" />Outras
        </div>
      </div>
      <div class="category_option" @click="finishedFilter">
        <div class="underline">
          <fa class="icon" :icon="['fas', 'chevron-right']" />Finalizadas
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, computed, getCurrentInstance } from "vue";

const instance = getCurrentInstance();

const todos = ref([]);

const allFilter = () => {
  if (instance) {
    instance.emit("all");
  }
};

const importantFilter = () => {
  if (instance) {
    instance.emit("important");
  }
};

const urgentFilter = () => {
  if (instance) {
    instance.emit("urgent");
  }
};

const othersFilter = () => {
  if (instance) {
    instance.emit("others");
  }
};

const finishedFilter = () => {
  if (instance) {
    instance.emit("finished");
  }
};

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});

const urgentCount = computed(() => {
  return todos.value.filter((todo) => todo.category === "urgent").length;
});

const importantCount = computed(() => {
  return todos.value.filter((todo) => todo.category === "important").length;
});
</script>

<style lang="stylus" scoped>
.categories
  display: flex
  flex-direction: column
  align-items: center
  justify-content: center
  height: 100%
  background-color: #f4fbff
  border-left: 1px solid #ccc
  width: 20%

  h2
    margin-bottom: 20px

.options
  display: flex
  flex-direction: column
  gap: 10px

.category_option
  display: flex
  align-items: center
  cursor: pointer
  color: #000
  transition: color 0.3s ease

  &:hover
    color: #007bff

.underline
  &:hover
    text-decoration: underline

.circle_span
  display: flex
  justify-content: center
  align-items: center
  border-radius: 50%
  width: 20px
  height: 20px
  margin-left: 8px
  color: #F4FBFF
  &.urgent
    background-color: #FF3B81
  &.important
    background-color: #FFC42E

.icon
  width: 14px
  height: 14px
  margin-right: 5px

span
  font-size: 14px
</style>
