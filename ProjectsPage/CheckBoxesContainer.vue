<template>
    <div class="checkboxes-container">
        <div class="checkbox-container">
            <input
                type="checkbox"
                id="option1"
                name="option1"
                :checked="checkBoxesOptions.currentProjects"
                @change="updateCheckbox('currentProjects',$event.target.checked)"
            />
            <label class="custom-checkbox" for="option1"></label>
            <span>Портфель проектов текущего года</span>
        </div>
        <div class="checkbox-container">
            <input
                type="checkbox"
                id="option2"
                name="option2"
                :checked="checkBoxesOptions.archievedProjects"
                @change="updateCheckbox('archievedProjects',$event.target.checked)"
            />
            <label class="custom-checkbox" for="option2"></label>
            <span>Архивные проекты</span>
        </div>
        <div class="checkbox-container">
            <input
                type="checkbox"
                id="option3"
                name="option3"
                :checked="checkBoxesOptions.projectInitiatives"
                @change="updateCheckbox('projectInitiatives',$event.target.checked)"
            />
            <label class="custom-checkbox" for="option3"></label>
            <span>Проект портфеля текущего года</span>
        </div>
    </div>
</template>

<script>
import { mapState, mapMutations } from 'vuex';

export default {
    computed: {
        ...mapState({
            checkBoxesOptions: state => state.checkBoxesOptions,
            // currentProjects: state => state.checkBoxesOptions.currentProjects,
            // archieveProjects: state => state.checkBoxesOptions.archieveProjects,
        }),
    },
    methods: {
        ...mapMutations({
        updateCheckboxesOptions: 'updateCheckBoxesOptions',
    }),
    updateCheckbox(option, value) {
        this.updateCheckboxesOptions({
            ...this.checkBoxesOptions,
            [option]: value,
        });
    },
    },
};
</script>

<style lang="scss" scoped>
@import "@/components/commonstyles.scss";

.checkbox-container {
    font-size: $smallSize;
    display: flex;
    align-items: center;
    margin: 10px 0;
    font-family: $font-family;
}

/* Скрываем стандартный екбокс */
input[type="checkbox"] {
    display: none;
}

/* Создаем стиль для кастомного чекбокса */
.custom-checkbox {
    width: 20px;
    height: 20px;
    border: 2px solid #ffa500; /* Оранжевая граница */
    border-radius: 4px; /* Закругленные углы */
    position: relative;
    cursor: pointer; /* Указатель при наведении */
    margin-right: 10px; /* Отступ справа от чекбокса */
}

/* Стиль для состояния "выбран" */
input[type="checkbox"]:checked + .custom-checkbox {
    background-color: #ffa500; /* Оранжевый фон */
}

/* Стиль для галочки */
.custom-checkbox::after {
    content: "";
    position: absolute;
    left: 5px;
    top: 2px;
    width: 5px;
    height: 10px;
    border: solid white; /* Цвет галочки */
    border-width: 0 2px 2px 0;
    transform: rotate(45deg);
    opacity: 0; /* Скрываем галочку по умолчанию */
}

/* Показ галочки при выборе чекбокса */
input[type="checkbox"]:checked + .custom-checkbox::after {
    opacity: 1; /* Показываем галочку */
}
</style>
