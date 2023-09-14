<template>
    <div>
        <p>これはCalculateTaxコンポーネントです</p>

        <div>
            <div v-for="i of rowCount" :key="i">
                <div class="input-row">
                    <div v-html="inputRow" v-for="inputRow of inputRows" :key="inputRow" ></div>
                </div>
            </div>
            <button @click="addRow">+</button>
            <button v-if="isMoreZero" @click="removeRow">-</button>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            rowCount: 3,
            inputRows: [
                '<input type="text" placeholder="品目" >',
                '<input type="number" placeholder="数量">',
                '<input type="number" placeholder="合計額">',
            ],
        };
    },
    methods: {
        addRow() {
            this.rowCount += 1;
        },
        removeRow() {
            this.rowCount -= 1;
        }
    },
    computed: {
        isMoreZero() {
            return this.rowCount > 0;
        },
    }
}
</script>

<style>
.input-row {
    display: flex;
}
</style>