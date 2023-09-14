<template>
    <div>
        <p>これはCalculateTaxコンポーネントです</p>

        <div>
            <button @click="addRow">列の追加</button>
            <button v-if="isMoreZero" @click="removeRow">列の削除</button>
            <div v-for="i of rowCount" :key="i">
                <div class="input-row">
                    <div v-html="inputRow" v-for="inputRow of inputRows" :key="inputRow" ></div>
                </div>
            </div>
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