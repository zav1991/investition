<template>
    <div
        :class="errorClass"
    >
        {{msg}}
    </div>
</template>

<script>
    export default {
        name: "Error",
        props: {
            type: String,
            msg: String,
        },
        computed: {
            errorClass: function() {
                return this.type ? `error error-${this.type}` : '';
            },
        },
    }
</script>

<style scoped>
    .error {
        background-color: rgb(230, 69, 69);
        box-shadow: 0px 1px 5px 0px rgba(0, 0, 0, 0.1);
        border-radius: 3px;
        position: absolute;
        font-size: 11px;
        font-family: "Arial";
        color: rgb(240, 242, 244);
        line-height: 16px;
        display: flex;
        text-align: left;
        align-items: center;
        text-wrap: normal;
        padding: 0px 8px;
        z-index: 2;
        margin-top: -43px;
        width: 111px;
        height: 47px;
    }

    .error::after {
        content: '';
        position: absolute;
        left: 9px;
        top: 47px;
        border: 4px solid transparent;
        border-top: 4px solid rgb(230, 69, 69);
    }

    .error-profit {
        margin-left: 130px;
    }

</style>