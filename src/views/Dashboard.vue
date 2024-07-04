<template>
  <div class="dashboard">
    <Sidebar />
    <Header/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import Sidebar from '../components/SideBar.vue'
import Header from '../components/Header.vue'

export default defineComponent({
  components: {
    Sidebar,
    Header
  },
})
</script>

<style scoped>
.dashboard {
  display: flex;
  width: 100%;
  height: 100vh;
}
</style>
