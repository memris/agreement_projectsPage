<template>
    <div class="search-container">
        <i class="search-icon">🔍</i>
        <input
        type="text"
      class="search-input"
      placeholder="Поиск... (введите имя или код)"
      v-model="localSearch"
      @input="$emit('input', localSearch)"
        />
    </div>
</template>

<script>
export default {
    props: {
        modelValue: {
            type: Object,
            required: true,
        }
    },
    data() {
        return {
            localSearch: this.modelValue?.ActionMenuOptions?.name || "", 
        };
    },
    watch: {
        // options: {
        //     handler(newValue) {
        //         this.localSearch = newValue?.ActionMenuOptions?.name || "";
        //     },
        //     deep: true,
        // },
        localSearch: function() {
            this.updateSearch(); // Вызываем обновление каждый раз при изменении поиска
        }
    },
    methods: {
        // updateSearch() {
        //     this.$emit("change", { ...this.options, ActionMenuOptions: {...this.options.ActionMenuOptions,name:this.localSearch} });
        // },
        updateSearch() {
    this.$emit("update:modelValue", {
      ...this.modelValue, 
      ActionMenuOptions: {
        ...this.modelValue.ActionMenuOptions, 
        name: this.localSearch,
      },
    });
    this.$emit('search', this.localSearch); 
    console.log("search emitted:", this.localSearch);
  },
    },
};
</script>

<style lang="scss" scoped>
@import "@/components/commonstyles.scss";

.search-container {
    position: relative;
    width: 300px; /* Ширина поля поиска */
}

.search-input {
    width: 100%;
    height: 100%;
    padding: 10px 40px 10px 40px; /* Отступы, чтобы оставить место для иконки */
    border: 2px solid #ccc; /* Цвет границы */
    border-radius: 25px; /* Закругленные углы */
    font-size: $smallSize; /* Размер шрифта */
    transition: border-color 0.3s; /* Плавный переход цвета границы */
}

.search-input:focus {
    border-color: #007bff; /* Цвет границы при фокусе */
    outline: none; /* Убираем стандартный контур */
}

.search-icon {
    position: absolute;
    left: 15px; /* Положение иконки */
    top: 50%; /* Центрирование по вертикали */
    transform: translateY(-50%); /* Центрирование по вертикали */
    color: #007bff; /* Цвет иконки */
}
</style>
