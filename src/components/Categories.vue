<template>
  <div class="categories">
    <div class="options">
      <h2>Categorias</h2>
      <div
        class="category_option"
        :class="{ selected: selectedCategory === 'all' }"
        @click="allFilter"
      >
        <div class="underline">
          <fa class="icon" :icon="['fas', 'chevron-right']" />Todas
        </div>
      </div>
      <div
        class="category_option"
        :class="{ selected: selectedCategory === 'urgent' }"
        @click="urgentFilter"
      >
        <div class="underline">
          <fa class="icon" :icon="['fas', 'chevron-right']" />Urgentes
        </div>
        <div class="circle_span urgent">
          <span>{{ urgentCount }}</span>
        </div>
      </div>
      <div
        class="category_option"
        :class="{ selected: selectedCategory === 'important' }"
        @click="importantFilter"
      >
        <div class="underline">
          <fa class="icon" :icon="['fas', 'chevron-right']" />Importantes
        </div>
        <div class="circle_span important">
          <span>{{ importantCount }}</span>
        </div>
      </div>
      <div
        class="category_option"
        :class="{ selected: selectedCategory === 'others' }"
        @click="othersFilter"
      >
        <div class="underline">
          <fa class="icon" :icon="['fas', 'chevron-right']" />Outras
        </div>
      </div>
      <div
        class="category_option"
        :class="{ selected: selectedCategory === 'finished' }"
        @click="finishedFilter"
      >
        <div class="underline">
          <fa class="icon" :icon="['fas', 'chevron-right']" />Finalizadas
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, ref, computed, getCurrentInstance } from "vue";

// Definição das propriedades recebidas pelo componente
const props = defineProps({
  todos: Array,
});

// Instância atual do componente
const instance = getCurrentInstance();

// Variável reativa para armazenar a categoria selecionada
const selectedCategory = ref("all");

// Funções para filtrar por cada categoria e emitir eventos
const allFilter = () => {
  selectedCategory.value = "all";
  if (instance) {
    instance.emit("all");
  }
};

const importantFilter = () => {
  selectedCategory.value = "important";
  if (instance) {
    instance.emit("important");
  }
};

const urgentFilter = () => {
  selectedCategory.value = "urgent";
  if (instance) {
    instance.emit("urgent");
  }
};

const othersFilter = () => {
  selectedCategory.value = "others";
  if (instance) {
    instance.emit("others");
  }
};

const finishedFilter = () => {
  selectedCategory.value = "finished";
  if (instance) {
    instance.emit("finished");
  }
};

// Computadas para contar o número de tarefas urgentes e importantes não finalizadas
const urgentCount = computed(() => {
  return props.todos.filter((todo) => todo.category === "urgent" && !todo.done)
    .length;
});

const importantCount = computed(() => {
  return props.todos.filter(
    (todo) => todo.category === "important" && !todo.done
  ).length;
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
  width: 230px

  h2
    margin-bottom: 30px
    font-size: 17px
    font-family: 'Gilroy-Bold'

.options
  display: flex
  flex-direction: column
  gap: 20px

.category_option
  display: flex
  align-items: center
  cursor: pointer
  color: #000
  transition: color 0.3s ease

  &.selected
    color: #007bff

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
