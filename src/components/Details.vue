<template>
    <details class="details">
        <summary class="details-summary">
            Ограничить прибыль и убыток
        </summary>
        <div class="details-wrapper">
            <Limits
                :limits="limits"
                v-model="limits"
            />
            <InputLimit
                label="Прибыль"
                detail="profit"
                :limits="limits"
                :sumInv="sumInv"
                :isValid="isValidProfit"
                :needValidate="needValidate"
                v-model="takeProfitData"
            />
            <InputLimit
                label="Убыток"
                detail="loss"
                :limits="limits"
                :sumInv="sumInv"
                :isValid="isValidLoss"
                :needValidate="needValidate"
                v-model="stopLossData"
            />
        </div>
    </details>
</template>

<script>
    import Limits from "./Limits";
    import InputLimit from "./InputLimit";

    export default {
        name: "Details",
        components: {InputLimit, Limits},
        props: {
            sumInv: Number,
            needValidate: Number,
        },
        data() {
            return {
                limits: false,//true - %, false - $
                takeProfitData: {},
                takeProfit: -1,
                stopLossData: {},
                stopLoss: -1,
                isValidProfit: true,
                isValidLoss: true,
            }
        },
        watch: {
            limits: function() {
                this.bubbleData();
            },
            takeProfit: function() {
                this.bubbleData();
            },
            stopLoss: function() {
                this.bubbleData();
            },
            needValidate: function() {
                this.validate();
            },
            takeProfitData: function() {
                this.takeProfit = this.takeProfitData.data;
                this.isValidProfit = this.takeProfitData.isValid;
            },
            stopLossData: function() {
                this.stopLoss = this.stopLossData.data;
                this.isValidLoss = this.stopLossData.isValid;
            },
        },
        methods: {
            bubbleData: function () {
                this.$emit('input', {
                        details: {limits: this.limits, takeProfit: this.takeProfit, stopLoss: this.stopLoss},
                        isValidDetails: this.isValidLoss && this.isValidProfit,
                    }
                )
            },
            validate: function () {},
        }
    }
</script>

<style scoped>
    .details {
        margin-top: 36px;
        height: 118px;
    }

    .details-summary {
        display: flex;
        font-size: 13px;
        font-family: "Arial";
        color: rgb(144, 146, 148);
        line-height: 22px;
        margin-left: 29px;
        outline: none;
        position: relative;
    }

    .details-summary::-webkit-details-marker {
        display: none;
    }

    .details-summary:before, .details-summary:after {
        content: '';
        position: absolute;
        left: -14px;
        bottom: 6px;
        border: 5px solid transparent;
        border-left: 5px solid rgb(144, 146, 148);
    }
    .details-summary:after {
        border-left: 5px solid white;
        left: -15px;
    }

    .details[open] > .details-summary:before, .details[open] > .details-summary:after {
        content: '';
        position: absolute;
        left: -16px;
        bottom: 3px;
        border: 5px solid transparent;
        border-top: 5px solid rgb(144, 146, 148);
    }
    .details[open] > .details-summary:after {
        border-top: 5px solid white;
        bottom: 4px;
    }

    .details-wrapper {}
</style>
