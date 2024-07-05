<template>
  <div class="sidebar">
    <img class="logo" src="../assets/images/logo.png" alt="logo" />
    <div class="container_options">
      <div
        class="option"
        :class="{ selected: selectedOption === 'dashboard' }"
        @click="selectOption('dashboard')"
      >
        <fa class="icon" :icon="['fas', 'table-columns']" /> Dashboard
      </div>
      <div
        class="option"
        :class="{ selected: selectedOption === 'tarefas' }"
        @click="selectOption('tarefas')"
      >
        <fa class="icon" :icon="['fas', 'circle-check']" /> Tarefas
      </div>
      <div
        class="option"
        :class="{ selected: selectedOption === 'configuracoes' }"
        @click="selectOption('configuracoes')"
      >
        <fa class="icon" :icon="['fas', 'gears']" /> Configurações
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const selectedOption = ref("dashboard");

const selectOption = (option: string) => {
  selectedOption.value = option;
};
</script>

<style lang="stylus" scoped>
.sidebar
  display: flex
  flex-direction: column
  align-items: center
  background-color: #2693ff
  border-right: 1px solid #ccc
  padding: 10px 0
  width: 150px
  font-family: 'Gilroy-Bold'

.logo
  width: 60px
  height: 60px
  border-radius: 15%

.container_options
  display: flex
  flex-direction: column
  justify-content: center
  height: 100%
  width: 100%

.option
  display: flex
  flex-direction: column
  align-items: center
  gap: 10px
  color: white
  text-align: center
  cursor: pointer
  box-sizing: border-box
  padding: 15px
  font-size: 14px

.icon
  width: 23px
  height: 23px

.selected
  background-color: #0d47a1
</style>
