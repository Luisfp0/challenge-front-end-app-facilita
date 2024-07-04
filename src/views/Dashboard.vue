<template>
  <div class="dashboard">
    <Sidebar />
    <h1>Dashboard</h1>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import Sidebar from '../components/SideBar.vue'

export default defineComponent({
  components: {
    Sidebar
  },
})
</script>

<style scoped>
.dashboard {
  display: flex;
  width: auto;
  height: 100vh;
}
</style>
