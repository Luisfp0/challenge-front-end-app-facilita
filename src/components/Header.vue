<template>
  <header class="dashboard-header">
    <div class="header-content">
      <div class="user-info">
        <img class="user-photo" src="../assets/images/user-photo.png" alt="User Photo" />
        <div class="user-details">
          <div class="user-name">Luís Fernando de Paulo</div>
          <div class="user-role">Front-end Developer</div>
        </div>
      </div>
    </div>
  </header>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
});
</script>

<style scoped>
.dashboard-header {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  width: 100%;
  height: 60px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 0 20px;
  z-index: 1000;
}

.header-content {
  display: flex;
  align-items: center;
}

.user-info {
  display: flex;
  align-items: center;
}

.user-photo {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin-right: 10px;
}

.user-details {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.user-name {
  font-size: 16px;
  font-weight: bold;
}

.user-role {
  font-size: 12px;
  color: #2693ff;
}
</style>
