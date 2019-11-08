<template>
    <div class="limits">
        <label class="limits-label">Ограничения в</label>
        <div class="limits-inputs-wrapper">
            <input
                type="radio"
                class="limits-input"
                id="limits-percent"
                name="limits"
                value="true"
                v-model="limit"
                @change="toggleLimits"
            >
            <label class="limits-input-label" for="limits-percent">%</label>
            <input
                type="radio"
                class="limits-input"
                id="limits-money"
                name="limits"
                value="false"
                v-model="limit"
                @change="toggleLimits"
            >
            <label class="limits-input-label" for="limits-money">$</label>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Limits",
        props: {
            limits: Boolean,
        },
        data() {
            return {
                limit: this.limits,//true - %, false - $
            }
        },
        methods: {
            toggleLimits: function() {
                console.log(`toggle. Current limits: ${this.limit}`);
                this.$emit('input', this.limit === 'true');
            },
        },
    }
</script>

<style scoped>
    .limits {
        display: flex;
        align-items: center;
        margin-top: 10px;
        font-size: 13px;
        font-family: "Arial";
        color: rgb(36, 37, 38);
        line-height: 22px;
        margin-left: 28px;
        margin-bottom: 4px;
    }

    .limits-label {}

    .limits-inputs-wrapper {}

    .limits-input {}

    .limits-input-label {}
</style>

