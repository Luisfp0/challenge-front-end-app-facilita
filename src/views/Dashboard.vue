<template>
  <div class="dashboard">
    <Sidebar />
    <div class="main_content">
      <Header />
      <Categories />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Sidebar from "../components/SideBar.vue";
import Header from "../components/Header.vue";
import Categories from "../components/Categories.vue";

export default defineComponent({
  components: {
    Sidebar,
    Header,
    Categories
  },
});
</script>

<style scoped>
.dashboard {
  display: flex;
  height: 100vh;
}

.main_content {
  display: flex;
  flex-direction: column;
  width: 100%;
}
</style>
