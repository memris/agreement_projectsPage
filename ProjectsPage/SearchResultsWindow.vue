<template>
    <div v-if="isOpen" class="modal">
      <div class="modal-content">
        <button @click="close">Закрыть</button>
        <h3>Результаты поиска:</h3>
        <ul>
          <li v-for="project in results" :key="project.id">
            {{ project.name }} ({{ project.code_name }})
          </li>
        </ul>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      results: Array,
      isOpen: Boolean
    },
    mounted() {
        console.log("SearchResultsWindow mounted:", this.results, this.isOpen);
    },
    methods: {
      close() {
        this.$emit('close');
      }
    }
  };
  </script>

  <style>
.modal {
  position: fixed; /* Фиксированное позиционирование */
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); /* Полупрозрачный фон */
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100; /* Убедитесь, что модальное окно выше других элементов */
}

.modal-content {
  background-color: #fff;
  padding: 20px;
  border-radius: 5px;
}</style>