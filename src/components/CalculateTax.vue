<template>
    <div>
        <p>これはCalculateTaxコンポーネントです</p>

        <div>
            <button @click="addRow">列の追加</button>
            <button v-if="isMoreZero" @click="removeRow">列の削除</button>
            <div v-for="(i, index) of rowCount" :key="i">
                <div class="input-row">
                    <input type="text" placeholder="品目" >
                    <input type="number" placeholder="数量">
                    <input type="number" placeholder="合計額" v-model="values[index]">
                </div>
            </div>
            <div>
                <p>レシートの合計額</p>
                <input type="number" >
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            rowCount: 3,
            values: ['', '', ''],
        };
    },
    methods: {
        addRow() {
            this.rowCount += 1;
            this.values.splice(this.rowCount - 1, 0, '');
        },
        removeRow() {
            this.rowCount -= 1;
            this.values.splice(this.rowCount - 1, 1);
        },
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