<template>
    <tr>
        <th></th>
        <th>
            <div>
                <input
                    type="text"
                    placeholder="Поиск..."
                    id="code"
                    @change="updateSearchButton($event.target)"
                />
                <button>↕️</button>
            </div>
        </th>
        <th>
            <div>
                <button>↕️</button>
            </div>
        </th>
        <th>
            <div>
                <input
                    type="text"
                    placeholder="Поиск..."
                    id="stage"
                    @change="updateSearchButton($event.target)"
                />
                <button>↕️</button>
            </div>
        </th>
        <th>
            <div>
                <button>↕️</button>
            </div>
        </th>
        <th>
            <div>
                <input
                    type="text"
                    placeholder="Поиск..."
                    id="realisation"
                    @change="updateSearchButton($event.target)"
                />
                <button>↕️</button>
            </div>
        </th>
        <th>
            <div>
                <input
                    type="text"
                    placeholder="Поиск..."
                    id="status"
                    @change="updateSearchButton($event.target)"
                />
                <button>↕️</button>
            </div>
        </th>
        <th>
            <div>
                <input
                    type="text"
                    placeholder="Поиск..."
                    id="ending"
                    @change="updateSearchButton($event.target)"
                />
                <button>↕️</button>
            </div>
        </th>
        <th>
            <div>
                <input
                    type="text"
                    placeholder="Поиск..."
                    id="manager"
                    @change="updateSearchButton($event.target)"
                />
                <button>↕️</button>
            </div>
        </th>
        <th>
            <div>
                <button>↕️</button>
            </div>
        </th>
    </tr>
</template>

<script>
/* eslint-disable */
export default {
    props: {
        options: {
            type: Object,
            required: true,
        },
    },
    data() {
        return {
            sortingOptions: { ...this.options },
        };
    },
    methods: {
        updateSearchButton(target) {
            sortingOptions[target.id] = target.value;
        },
    },
};
</script>

<style lang="scss" scoped>
@import "@/components/tableStyles.scss";
</style>
