<template>
    <div>
        <p>これはCalculateTaxコンポーネントです</p>
        <div>
            <div>
                <button @click="switchTax">税率切り替え</button>
                <p>現在の税率は{{taxRate}}%です</p>
            </div>
            <button @click="addRow">列の追加</button>
            <button v-if="isMoreZero" @click="removeRow">列の削除</button>
            <div v-for="(i, index) of rowCount" :key="i">
                <div class="input-row">
                    <input type="text" v-model.number="items[index]">
                    <input type="number" v-model.number="quantity[index]">
                    <input type="number" v-model.number="values[index]">
                </div>
            </div>
            <div>
                <p>インプットの合計額：{{sumValues}}</p>
                <p>インプットに税率をかけた合計額：{{taxedSumValues}}</p>
                <p>品目ごとの税込価格の合計額：{{sumRoundedValues}}</p>
                <p>レシートの合計額</p>
                <input type="number" v-model.number="inputSum">
            </div>
            <button @click="calculateTaxedValues()">税込価格を計算する</button>
            <button @click="calculateRoundedValues()">税込価格の端数を丸める</button>
            <button @click="adjustValues()">計算する</button>
            <button @click="check()">確認する</button>
            <button @click="showOutput()">output表示切り替え</button>
            <div v-show="output" v-for="(item, index) of items" :key="item">
                <div class="output-row">
                    <input type="text" v-model="items[index]">
                    <input type="number" v-model.number="taxedValues[index]" readonly>
                    <input type="number" v-model.number="roundedValues[index]" readonly>
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
            items: ['品物1', '品物2', '品物3'],
            quantity: [0, 0, 0],
            values: [0, 0, 0],
            taxedValues :[],
            roundedValues:[],
            taxRate: 8,
            tax: true,
            inputSum: 0,
            output: false,
        };
    },
    methods: {
        addRow() {
            this.rowCount += 1;
            this.items.splice(this.rowCount, 0, '品物' + this.rowCount);
            this.quantity.splice(this.rowCount, 0, 0);
            this.values.splice(this.rowCount, 0, 0);
        },
        removeRow() {
            this.rowCount -= 1;
            this.items.splice(this.rowCount, 1);
            this.quantity.splice(this.rowCount, 1);
            this.values.splice(this.rowCount, 1);
        },
        switchTax() {
            this.tax = !this.tax;
            if (this.tax === true) {
                this.taxRate = 8;
            } else {
                this.taxRate = 10;
            }
        },
        /* 下記の2つの処理を分割するのは、品目ごとの税別価格を調整する際に端数ありの価格が必要になるため */
        calculateTaxedValues() {
            this.taxedValues = [];
            this.values.forEach((value) => {
                this.taxedValues.push(this.roundDown(this.addTax(value)));
            });
        },
        calculateRoundedValues() {
            this.roundedValues = [];
            this.taxedValues.forEach((taxedValue) => {
                this.roundedValues.push(Math.round(taxedValue));
            })
        },
        adjustValues() {
            this.calculateTaxedValues();
            this.calculateRoundedValues();
            // 等しい場合は調整せずに出力
            if ( this.taxedSumValues === this.sumRoundedValues ) {
                console.log('値を調整しません');
            } else {
                console.log('値を調整します');
            }
        },
        addTax(e) {
            return e * (1 + this.taxRate / 100);
        },
        roundDown(e) {
            return Math.floor(e * 100) / 100; // 入力値の小数点第３位以下を切り捨て
        },
        check() {
            console.log(this.items);
            console.log(this.quantity);
            console.log(this.values);
            console.log(this.taxedValues);
            console.log(this.roundedValues);
        },
        showOutput() {
            this.output = !this.output;
        }
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
        taxedSumValues() {
            return Math.floor(this.addTax(this.sumValues));
        },
        sumRoundedValues() {
            const sumRoundedValue = this.roundedValues.reduce((sumRoundedValue, value) => {
                return sumRoundedValue + value;
            }, 0);
            return sumRoundedValue;
        },
    }
}
</script>

<style>
.input-row {
    display: flex;
}
</style>