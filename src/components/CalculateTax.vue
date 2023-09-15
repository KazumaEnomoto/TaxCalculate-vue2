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
                    <input type="number" placeholder="0" v-model.number="values[index]">
                </div>
            </div>
            <div>
                <p>インプットの合計額：{{sumValues}}</p>
                <p>インプットに税率をかけた合計額：{{taxAddedValues}}</p>
                <p>レシートの合計額</p>
                <input type="number" >
            </div>
        </div>
        <button @click="check()">確認する</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            rowCount: 3,
            values: [0, 0, 0],
            taxRate: 1.08,
        };
    },
    methods: {
        addRow() {
            this.rowCount += 1;
            this.values.splice(this.rowCount - 1, 0, 0);
        },
        removeRow() {
            this.rowCount -= 1;
            this.values.splice(this.rowCount - 1, 1);
        },
        check() {
            console.log(this.values);
        },
    },
    computed: {
        // 入力列削除ボタンの表示切り替え基準
        isMoreZero() {
            return this.rowCount > 0;
        },
        sumValues() {
            const sumValue = this.values.reduce((sumValue, value) => {
                return sumValue + value;
            }, 0);
            return sumValue;
        },
        taxAddedValues() {
            // 少数内における微妙な誤差の除去
            return Math.floor(this.sumValues * this.taxRate * 100) / 100; 
        }
    }
}
</script>

<style>
.input-row {
    display: flex;
}
</style>