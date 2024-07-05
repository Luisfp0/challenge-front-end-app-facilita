<template>
  <div class="container_delete">
    <div class="task_container">
      <div class="background_icon_trash">
        <fa class="icon_trash" :icon="['fas', 'trash-can']" />
      </div>
      <h1>Tem certeza que deseja <span>excluir</span> esta tarefa ?</h1>
      <h2>Esta ação não poderá ser desfeita.</h2>
      <div class="actions">
        <div class="buttons_container">
          <button class="cancel_button" @click="cancelDelete">Cancelar</button>
          <button class="delete_button" @click="confirmDelete">Confirmar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { getCurrentInstance, onMounted } from "vue";

const instance = getCurrentInstance();

const cancelDelete = () => {
  if (instance) {
    instance.emit("close");
  }
};

const confirmDelete = () => {
  if (instance) {
    instance.emit("delete");
  }
};
</script>

<style lang="stylus" scoped>
.container_delete
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
  width: 350px
  background-color: #ffffff
  padding: 40px
  border: 1px solid #ccc
  border-radius: 5px
  display: flex
  flex-direction: column
  align-items: center
  justify-content: center

.background_icon_trash
  background-color: #edf6fc
  width: 120px
  height: 120px
  display: flex
  justify-content: center
  align-items: center
  border-radius: 50%
  margin-bottom: 20px

.icon_trash
  color: rgba(0, 0, 0, 0.747)
  width: 50px
  height: 50px

.actions
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center
  gap: 10px

.task_container span
  color: red

.buttons_container
  display: flex
  gap: 10px

.cancel_button
  width: 100px
  background-color: #1ad18f
  padding: 10px 15px
  font-size: 13px
  color: #ffffff
  border: none
  border-radius: 5px

.delete_button
  width: 100px
  background-color: #ff4874
  padding: 10px 15px
  font-size: 13px
  color: #ffffff
  border: none
  border-radius: 5px

.task_container h2
  margin-bottom: 25px
</style>
