<template>
  <div class="categories">
    <h2>Categorias</h2>
    <div class="options">
      <div class="category-option">
        <fa class="icon" :icon="['fas', 'chevron-right']" />Todas
      </div>
      <div class="category-option">
        <fa class="icon" :icon="['fas', 'chevron-right']" />Urgentes
      </div>
      <div class="category-option">
        <fa class="icon" :icon="['fas', 'chevron-right']" />Importantes
      </div>
      <div class="category-option">
        <fa class="icon" :icon="['fas', 'chevron-right']" />Outras
      </div>
      <div class="category-option">
        <fa class="icon" :icon="['fas', 'chevron-right']" />Finalizadas
      </div>
    </div>
  </div>
</template>

<style scoped>
.categories {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 10%;
  height: 100vh;
  background-color: #f0f0f0;
  padding: 20px;
  border-left: 1px solid #ccc;
}

.category-option {
  color: #000;
  margin: 5px 0;
  cursor: pointer;
}

.category-option:hover {
  color: #007bff;
  text-decoration: underline;
}

.icon {
  width: 14px;
  height: 14px;
  margin-right: 5px;
}

h2 {
  margin-bottom: 30px;
}

.options {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>
