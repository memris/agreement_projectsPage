<template>
  <tbody> 
    <template v-if="groupingByStage">
      <template v-for="(value, index) in data" :key="value.code_name">
        <tr colspan="9" class="projects-container" :index="index">
          <td>{{ index + 1 }}</td> 
          <td>{{ value.code_name }}</td>
          <td class="td-name">{{ value.name }}</td>
          <td>{{ value.stage_description }}</td>
          <td>{{ value.priority }}</td>
          <td>{{ value.realisation_percent }}%</td>
          <td>{{ value.status_description }}</td>
          <td>{{ value.current_end }}</td>
          <td class="td-shortDescription">{{ value.short_description }}</td>
        </tr>
      </template>
    </template>
    <template v-else>
      <tr v-for="(value, index) in data" :key="value.code" class="projects-container">
        <td>{{ index + 1 }}</td> 
        <td>{{ value.code_name }}</td>
        <td class="td-name">{{ value.name }}</td>
        <td>{{ value.stage_description }}</td>
        <td>{{ value.priority }}</td>
        <td>{{ value.realisation_percent }}%</td>
        <td>{{ value.status_description }}</td>
        <td>{{ value.current_end }}</td>
        <td class="td-shortDescription">{{ value.short_description }}</td>
      </tr>
    </template>
  </tbody>
</template>
  
  <script>
  export default {
    props: {
      data: {
        type: Array,
        required: true,
      },
      groupingByStage: {
        type: Boolean,
        required: true,
      },
      groupedData: {
        type: Object,
        default: null,
      },
      groupingCriteria: {
        type: String,
        default: null,
      },
    },
    methods: {
      getIndex(index) {
        if (this.groupingByStage) {
          let overallIndex = 0;
          for (let key in this.groupedData) {
            if (this.data === this.groupedData[key]) {
              overallIndex += index + 1;
              break;
            } else {
              overallIndex += this.groupedData[key].length;
            }
          }
          return overallIndex;
        }
      },
    },
  };
  </script>

<style lang="scss" scoped>
@import "@/components/tableStyles.scss";
</style>
