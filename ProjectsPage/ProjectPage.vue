<template>
  <div class="project-page-container">
    <div class="first-container" v-if="currentTable === 'projects'">
      <div class="page-name">Реестр проектов</div>
      <CheckBoxesContainer v-model="options"></CheckBoxesContainer>
    </div>

    <ActionTableMenu
      :value="options"
      @update:ActionMenuOptions="handleActionMenuChange"
      @search="handleSearch"
    />

    <div class="search-container">
      <button @click="performSearch">Найти</button>
    </div>

    <SearchResultsWindow
      :results="filteredSearchResults"
      :isOpen="showSearchResults"
      @close="closeSearchResults"
    />

    <div class="table-container">
      <button @click="toggleTable('projects')">Общая информация</button>
      <br>
      <button @click="toggleTable('deadlines')">Сроки</button>

      <ProjectsTable v-if="currentTable === 'projects'"></ProjectsTable>
      <DeadlinesTable 
        v-if="currentTable === 'deadlines'" 
        :data="deadlinesData" 
        :groupingByStage="false" 
      ></DeadlinesTable>
    </div>
  </div>
</template>

<script>
import ProjectsTable from "./ProjectsTable.vue";
import CheckBoxesContainer from "./CheckBoxesContainer.vue";
import ActionTableMenu from "./ActionTableMenu.vue";
import { mapGetters, mapMutations } from "vuex";
import SearchResultsWindow from "./SearchResultsWindow.vue";
import DeadlinesTable from "../DeadlinesPage/DeadlinesTable.vue";

export default {
  components: {
    ProjectsTable,
    CheckBoxesContainer,
    ActionTableMenu,
    SearchResultsWindow,
    DeadlinesTable,
  },
  data() {
    return {
      options: {
      ActionMenuOptions: this.$store.state.ActionMenuOptions || [],
    },
    currentTable: 'projects',
    checkBoxesOptions: {
      currentProjects: true,
      archieveProjects: false,
    },
    searchQuery: '', // Поле для поиска
    showSearchResults: false,
  //  searchResults: [], 
    tableDataLoaded: false, 
  };
  },
  computed: {
    ...mapGetters(['filteredAndSortedTableData', 'deadlinesData']),

    filteredSearchResults() {
  if (!this.searchQuery || typeof this.searchQuery !== 'string') {
    return this.filteredAndSortedTableData; 
  }

  const searchTerm = this.searchQuery.trim().toLowerCase();
  return this.filteredAndSortedTableData.filter(item => {
    return item.name.toLowerCase().includes(searchTerm) ||
           item.code_name.toLowerCase().includes(searchTerm);
  });
},


  },
  watch: {
      filteredAndSortedTableData(newValue) {
  console.log("filteredAndSortedTableData updated:", newValue); 
  this.tableDataLoaded = newValue.length > 0; 
},
},
  methods: {
    toggleTable(table) {
      this.currentTable = table;
    },
      handleSearch(query) {
console.log("handleSearch called:", query);
this.searchQuery = query;
},
...mapMutations(['updateSearchQuery']),
  performSearch() {
      console.log("tableDataLoaded:", this.tableDataLoaded);
      console.log("searchQuery:", this.searchQuery);
      if (this.searchQuery && typeof this.searchQuery === 'string') {
        this.showSearchResults = true;
  this.searchResults = this.filteredAndSortedTableData.filter(item => {
    const searchTerm = this.searchQuery.trim().toLowerCase();
    return item.name.toLowerCase().includes(searchTerm) ||
           item.code_name.toLowerCase().includes(searchTerm);
  });
  console.log("searchResults:", this.searchResults); 
  if (this.filteredSearchResults.length === 0) { 
          alert('Нет результатов для отображения');
          this.showSearchResults = false; 
        }
      }
},


  closeSearchResults() {
    this.showSearchResults = false;
    this.searchResults = []; // Очищаем результаты
  },
  handleCheckboxChange(newValue) {
    this.$store.commit('updateCheckBoxesOptions', newValue);
  },
  handleActionMenuChange(newValue) {
    this.$store.commit('updateActionMenuOptions', newValue);
  },
},
mounted() {
  console.log('mounted');
  const pageRoute = '/projectsList'; 
  this.$store.dispatch('fetchTableData', pageRoute);
  this.$store.dispatch('fetchDeadlinesData');
},
};
</script>

<style lang="scss" scoped>
@import "@/components/commonstyles.scss";

* {
  padding: 0;
  margin: 0;
  border: none;
}

.project-page-container {
  width: 100%;
  margin-top: 100px;
  padding-top: 25px;
  display: flex;
  align-items: center;
  flex-direction: column;
  gap: 15px;
}

.search-container {
  padding-right: 800px;
display: flex;
text-decoration:underline;

}

.first-container {
  min-height: 100px;
  width: $width-table-pages;
  display: flex;
  flex-direction: row;
  gap: 100px;
}

.page-name {
  font-size: $headSize;
  font-family: $font-family;
}

/* модальное окно */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-content {
  background: #fff;
  padding: 20px;
  border-radius: 5px;
  width: 50%;
}

.modal-content h3 {
  margin-bottom: 10px;
}

.modal-content button {
  margin-top: 10px;
}
</style>
