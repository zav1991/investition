<template>
    <div class="header">{{msg}}</div>
</template>

<script>
    export default {
        name: "InvestmentHeader",
        data() {
            return {
                msg: 'Инвестировать сейчас',
            }
        },
    }
</script>

<style scoped>
    .header {
        height: 45px;
        border-bottom: 1px solid rgb(224, 226, 228);
        display: flex;
        align-items: center;
        padding-left: 25px;
        font-size: 16px;
        line-height: 32px;
        font-family: "Roboto Medium";
}
</style>
