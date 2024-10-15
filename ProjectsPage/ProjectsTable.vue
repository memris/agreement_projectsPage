<template>
  <div v-if="!filteredAndSortedTableData">Загрузка данных...</div>
  <div v-else-if="filteredAndSortedTableData.length === 0">
    Нет данных для отображения
  </div>
  <table v-else>
    <TableHeader></TableHeader>
    <!-- <TableButtonsHeader v-model="optionsButton"></TableButtonsHeader>  -->
    <template v-if="isGrouped">
      <template v-for="(group, key) in groupedData" :key="key">
        <tr class="group-header">
          <td>{{ key }}</td>
        </tr>
        <ProjectsTableData
          :data="group"
          :groupingByStage="isGrouped"
          :groupingCriteria="groupingCriteria"
        ></ProjectsTableData>
      </template>
    </template>
    <template v-else>
      <ProjectsTableData
        :data="filteredAndSortedTableData"
        :groupingByStage="isGrouped"
        :groupingCriteria="groupingCriteria"
      ></ProjectsTableData>
    </template>
  </table>
</template>
<script>
import ProjectsTableData from "./ProjectsTableData.vue";
//import TableButtonsHeader from "./TableButtonsHeader.vue";
import TableHeader from "./TableHeader.vue";
import { mapGetters, mapState } from "vuex";

export default {
    components: {
        TableHeader,
        ProjectsTableData,
        //TableButtonsHeader,
    },
    computed: {
        ...mapGetters(['filteredAndSortedTableData']),
        ...mapState(['isGrouped','groupingCriteria']),
        groupedData() {
            if (!this.isGrouped || !this.groupingCriteria) return {};

            console.log('groupedData - groupingCriteria:', this.groupingCriteria); 
            console.log('groupedData - filteredAndSortedTableData:', this.filteredAndSortedTableData); 

            return this.filteredAndSortedTableData.reduce((groups,item) => {
                // const stage = item.stage;
                // if (!groups[stage]) {
                //     groups[stage] = [];
                // }
                // groups[stage].push(item);
                const groupKey = item[this.groupingCriteria];
                if(!groupKey){
                  console.warn('Group key is indefined for item:',item);
                  return groups;
                }
                if (!groups[groupKey]) {
                  groups[groupKey] = [];
                }
                groups[groupKey].push(item);
                return groups;
            }, {});
        },
        // groupingByStage() {
        //     return this.$store.state.isGrouped;
        // },
    },
};
</script>

<style lang="scss" scoped>
@import "@/components/tableStyles.scss";
</style>
