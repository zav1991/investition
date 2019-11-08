<template>
    <div class="investment">
        <Header />
        <SumInv
            :sumInv="sumInv"
            :needValidate="needValidate"
            :isValidSum="isValidSum"
            v-model="sumData"
        />
        <div class="mult-line">
            <Multiplier
                :mult="mult"
                :needValidate="needValidate"
                :isValidMult="isValidMult"
                v-model="multData"
            />
            <ResultSum :result="result"/>
        </div>
        <Details
            :sumInv="sumInv"
            :needValidate="needValidate"
            v-model="detailsData"
        />
        <div class="buttons-wrapper">
            <div
                class="button-in-down"
                @click="sendRequest('reduction')"
            >
                <div class="button-in-down--left-part">
                    <img src="../assets/ArrowDown.png" alt="">
                </div>
                <div class="button-in-down--right-part">В снижение</div>
            </div>
            <div
                class="button-in-up"
                @click="sendRequest('growth')"
            >
                <div class="button-in-up--left-part">
                    <img src="../assets/ArrowUp.png" alt="">
                </div>
                <div class="button-in-up--right-part">В рост</div>
            </div>
        </div>
    </div>
</template>

<script>
    import Header from "./Header";
    import SumInv from "./SumInv";
    import Multiplier from "./Multiplier";
    import ResultSum from "./ResultSum";
    import Details from "./Details";

    export default {
        name: "Investment",
        components: {Details, ResultSum, Multiplier, SumInv, Header},
        data() {
            return {
                sumData: {},
                sumInv: 5000,
                isValidSum: true,
                multData: {},
                mult: 40,
                isValidMult: true,
                detailsData: {},
                details: {},
                isValidDetails: true,
                needValidate : 0,
            }
        },
        computed: {
            result: function() {
                const t = this;
                return t.sumInv * t.mult;
            },
        },
        watch: {
            sumData: function() {
                this.sumInv = this.sumData.sumInv;
                this.isValidSum = this.sumData.isValidSum;
            },
            multData: function() {
                this.mult = this.multData.mult;
                this.isValidMult = this.multData.isValidMult;
            },
            detailsData: function() {
                this.details = this.detailsData.details;
                this.isValidDetails = this.detailsData.isValidDetails;
            },
        },
        methods: {
            sendRequest: async function(direction) {
                const t = this;
                t.needValidate++;
                let sendData = {
                    sumInv: t.sumInv,
                    mult: t.mult,
                    direction: direction
                };
                if (t.details.takeProfit && t.details.takeProfit !== -1) {
                    if(t.details.limits) {//in percent
                        sendData.takeProfit = t.sumInv / 100 * t.details.takeProfit;
                    } else {
                        sendData.takeProfit = t.details.takeProfit;
                    }
                }
                if (t.details.stopLoss && t.details.stopLoss !== -1) {
                    if(t.details.limits) {//in percent
                        sendData.stopLoss = t.sumInv / 100 * t.details.stopLoss;
                    } else {
                        sendData.stopLoss = t.details.stopLoss;
                    }
                }
                if (this.isValidSum && this.isValidMult && this.isValidDetails) {
                    fetch('http://127.0.0.1:8080', {
                        method: 'POST',
                        mode: 'no-cors',
                        headers: {
                            'Content-Type': 'application/json',
                        },
                        body: JSON.stringify(sendData),
                    }).then(() => console.log("Запрос отправлен"))
                        .catch(() => console.log("Возникла ошибка при отправке"));
                }
            },
        },
    }
</script>

<style scoped>
    .investment {
        width: 316px;
        height: 368px;
        background-color: white;
    }
    .mult-line {
        display: flex;
    }

    .buttons-wrapper {
        display: flex;
        justify-content: center;
        margin-top: 21px;
    }

    .button-in-down, .button-in-up {
        border-radius: 4px;
        box-shadow: inset 30px 0px 0px 0px rgba(233, 95, 95, 0.004);
        width: 128px;
        height: 44px;
        display: flex;
        user-select: none;
    }

    .button-in-down {
        background-color: rgb(233, 95, 95);
    }

    .button-in-down:active {
        background-color: rgb(242, 102, 102);
    }

    .button-in-down:active > :nth-child(2) {
        background-color: rgb(233, 95, 95);
    }

    .button-in-up {
        background-color: rgb(73, 156, 56);
        margin-left: 8px;
    }

    .button-in-up:active {
        background-color: rgb(83, 166, 66);
    }

    .button-in-up:active > :nth-child(2) {
        background-color: rgb(73, 156, 56);
    }

    .button-in-down--left-part, .button-in-up--left-part {
        width: 30px;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .button-in-down--right-part, .button-in-up--right-part {
        width: 98px;
        height: 100%;
        border-radius: 4px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: white;
        font-size: 13px;
        font-family: "Arial";
        line-height: 22px;
    }

    .button-in-down--right-part {
        background-color: rgb(242, 102, 102);
    }

    .button-in-up--right-part {
        background-color: rgb(83, 166, 66);
    }

</style>
