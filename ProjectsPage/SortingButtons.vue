<template>
    <div class="container">
        <button @click="resetGrouping()"
        class="button-home">
            <div>
                <img src="@/assets/home_icon.svg" class="home-icon" alt="">
            </div>
        </button>
       <div class="dropdown">
        <button class="gray-button grouping-button"
        @click="showDropdown = !showDropdown">Группировка...</button>
        <ul v-if="showDropdown" class="dropdown-menu">
        <li 
        v-for="stage in stages"
        :key="stage"
        @click="groupingByStage(stage)"> {{  stage }}
        </li>    
        </ul>
       </div> 
        <ExportButton class="gray-button"></ExportButton>
    </div>
</template>

<script>
import {mapMutations } from 'vuex';
import ExportButton from './ExportButton.vue';





export default {
    components: {
        ExportButton,
    },
    data() {
        return {
            showDropdown: false,
        //     stages: [
        //     "Предпроект",
        // "Реализация",
        // "Завершен",
        // "Закрыт",
        // "Отменен",
        // "Приостановлен",
        // "Инициация",
        // "Заказчик проекта",
        //     ],
        };
    },
    computed: {
        stages() {
            return this.$store.state.stages;
        }
    },
    methods: {
        
        ...mapMutations(['groupingByStage']),
        groupingByStage(stage) {
            if(!this.$store.state.tableData.some(item => item['stage_description'] === stage)) {
                console.warn('No data found for grouping by stage:',stage);
                return;
            }
            console.log('Selected stage for grouping:',stage);
            this.$store.commit('setGroupingCriteria', stage);
            this.showDropdown = false;
        },
        resetGrouping() {
            this.$store.commit('resetGrouping');
        },

        
    },
};
</script>

<style lang="scss" scoped>
@import "@/components/commonstyles.scss";

.dropdown .gray-button:first-of-type { /* Применяем стили только к первой кнопке */
    padding: 9.5px 20px; 
    font-size: 10px; 
}

.grouping-button {
    padding: 10px 20px; 
    font-size: 14px; 
}

.dropdown {
    position: relative;
    display: inline-block;
}
.dropdown-menu {
    position: absolute;
    top: 100%;
    left: 0;
  z-index: 1000;
  display: none;
  padding: 10px;
  background-color: #fff;
  border: 1px solid #ccc;
  list-style: none;
}
.dropdown:hover .dropdown-menu {
    display: block;
}
.dropdown-menu li{
    padding: 5px;
    cursor: pointer;
}
.dropdown-menu li:hover {
    background-color: #f0f0f0;
}

.container {
    gap: 20px;
}

.gray-button {
    font-size: $smallSize;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #ccc; /* Серый фон */
    border: none; /* Без границы */
    color: #333; /* Темно-серый текст */
    padding: 15px 30px; /* Отступы */
    font-size: $smallSize; /* Размер шрифта */
    border-radius: 20px; /* Закругленные углы */
    cursor: pointer; /* Указатель при наведении */
    transition: background-color 0.3s, transform 0.2s; /* Плавный переход */
}

.gray-button:hover {
    background-color: #bbb; /* Более темный серый фон при наведении */
}

.gray-button:active {
    transform: scale(0.95); /* Уменьшение размера при нажатии */
}

.button-home {
    background-color: #fff;
    border: 2px solid $mainColor;
    border-radius: 8px;
    padding: 10px 15px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: background-color 0.3s, transform 0.2s;
}
.home-icon {
    scale: 120%;
}

.button-home:hover {
    background-color: #f0f0f0;
    transform: scale(1.05);
}
</style>
