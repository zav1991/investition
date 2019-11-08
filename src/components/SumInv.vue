<template>
    <div class="sum">
        <div class="sum-label">{{label}}</div>
        <div class="sum-input-wrapper">
            <div class="sum-input-currency">$</div>
            <input
                :class="inputClass"
                type="text"
                v-model="summ"
                @keypress="validationInput"
                @input="changeSum"
                @focus="onFocus"
                onpaste="return false"
            />
            <Error
                v-if="needError"
                type="sum"
                msg="Минимальная сумма инвестиции $ 100"
            />
        </div>
    </div>
</template>

<script>
    import Error from "./Error";

    export default {
        name: "SumInv",
        components: {Error},
        props: {
            sumInv: Number,
            isValidSum: Boolean,
            needValidate: Number,
        },
        data() {
            return {
                label: 'Сумма инвестиции',
                summ: this.sumInv.toLocaleString('ru-RU'),
                needError: false,
            };
        },
        computed: {
            inputClass: function() {
                return !this.needError ? 'sum-input' : 'sum-input sum-input-error'
            },
        },
        watch: {
            needValidate: function() {
                this.needError = !this.isValidSum;
            },
        },
        methods: {
            validationInput: function(event) {
                if ( !/\d/.test( event.key ) ) {
                    event.preventDefault();
                    return false;
                }
            },
            changeSum: function() {
                const currentSum = parseInt(this.summ.replace(/\s/g, ''));
                if (currentSum < 0) {
                    this.summ = '0';
                } else if(currentSum > 200000) {
                    this.summ= '200 000';
                } else if(this.summ === '') {
                    this.summ = '0';
                } else {
                    this.summ = currentSum.toLocaleString('ru-RU');
                }
                this.validate();
            },
            onFocus: function() {
                this.needError = false;
                this.validate();
            },
            validate: function() {
                this.$emit('input', {
                    sumInv: parseInt(this.summ.replace(/\s/g, '')),
                    isValidSum: parseInt(this.summ.replace(/\s/g, '')) >= 100
                });
            },
        },
    }
</script>

<style scoped>
    .sum {
        display: flex;
        justify-content: stretch;
        margin-top: 20px;
    }

    .sum-label {
        width: calc(50% - 29px);
        font-family: 'Arial Regular';
        font-size: 13px;
        line-height: 22px;
        display: flex;
        padding-left: 29px;
        align-items: center;
    }

    .sum-input-wrapper {
        display: flex;
        align-items: center;
    }

    .sum-input-currency {
        position: absolute;
        margin-top: 1px;
        margin-left: 8px;
        z-index: 1;
        font-family: "Arial Regular";
        font-size: 13px;
        line-height: 22px;
        color: rgb(144, 146, 148);
    }

    .sum-input {
        text-align: end;
        font-family: "Arial Regular";
        font-size: 13px;
        line-height: 22px;
        border: 1px solid rgb(192, 194, 196);
        border-radius: 3px;
        margin-right: 23px;
        padding: 8px 10px;
        width: 106px;
        height: 10px;
        position: relative;
    }

    .sum-input-error {
        border-color: rgb(230, 69, 69);
    }
</style>