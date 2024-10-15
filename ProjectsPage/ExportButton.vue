<template>
    <button @click="exportToExcel">Экспорт...</button>
</template>

<script>
import {mapGetters} from 'vuex';
import * as XLSX from "xlsx"; // Импортируем библиотеку XLSX

    export default{
        data(){
            return {
                tableHeaders: ['Код', 'Название проекта', 'Этап', 'Приоритет', 'Реализация', 'Статус', 'Завершение', 'Описание']
            }
        },
        name: 'ExportButton',
        methods: {
            ...mapGetters(['filteredAndSortedTableData']),
            //Мишино
            exportToExcel(){
                const dataHeaders = [this.tableHeaders];
                const dataToExport = this.filteredAndSortedTableData(this.$state)
                .map(item => [item.code_name, item.name, item.stage_description, item.priority, item.realisation_percent, item.status_description, item.current_end, item.short_description])

                const worksheet = XLSX.utils.aoa_to_sheet(dataHeaders.concat(dataToExport))
                const workbook = XLSX.utils.book_new();

                // Добавляем лист в книгу
                XLSX.utils.book_append_sheet(workbook, worksheet, "Таблица");
                // Генерируем файл Excel и предлагаем его скачать
                XLSX.writeFile(workbook, "projectList.xlsx");
            },
            //
        }
    };
</script>
