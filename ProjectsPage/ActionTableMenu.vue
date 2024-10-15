<template>
    <div class="container">
        <SearchBar :modelValue="options" @update:modelValue="updateOptions" @search="$emit('search', $event)" /> 
        <SortingButtons :menu-options="ActionMenuOptions"></SortingButtons>
    </div>
</template>

<script>
import SearchBar from "./SearchBar.vue";
import SortingButtons from "./SortingButtons.vue";
export default {
    components: {
        SearchBar,
        SortingButtons,
    },
    props: {
        value: {
            type: Object,
            required: true,
        },
    },
    data() {
        return {
            ActionMenuOptions: this.value?.ActionMenuOptions || [], 
            options: { ...this.value },
        };
    },
    watch: {
        value: {
            handler(newValue) {
                this.ActionMenuOptions = newValue?.ActionMenuOptions || [];
                this.options = { ...newValue };
            },
            deep: true, 
        },
    },
    methods: {
        updateOptions(newOptions) {
            this.options = newOptions;
            this.$emit("update:value", newOptions);
        }
    },
};
</script>

<style lang="scss" scoped>
@import "@/components/commonstyles.scss";

.container {
    width: $width-table-pages;
    display: flex;
    justify-content: start;
    height: 30px;
    gap: 20px;
}
</style>
