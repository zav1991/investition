<template>
    <div class="result-sum">=&ensp;$ {{result.toLocaleString('ru-RU')}}</div>
</template>

<script>
    export default {
        name: "ResultSum",
        props: {
            result: Number,
        },
    }
</script>

<style scoped>
    .result-sum {
        display: flex;
        align-items: center;
        margin-top: 10px;
        margin-left: 5px;
        font-size: 13px;
        font-family: "Arial";
        color: rgb(144, 146, 148);
        line-height: 22px;
    }
</style>
