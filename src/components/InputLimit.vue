<template>
    <div class="limit">
        <input
            type="checkbox"
            class="limit-checkbox"
            id="limit-checker"
            v-model="isActive"
        >
        <label class="limit-checkbox-label" for="limit-checker">{{label}}</label>
        <div class="limit-currency">{{currency}}</div>
        <input
            type="number"
            :class="inputClass"
            min="0"
            step="1"
            name="limit"
            :readonly="readonly"
            @keypress="validationInput"
            v-model="limit"
            onpaste="return false"
            @focus="onFocus"
            @input="changeLimit"
        >
        <Error
            v-if="needError"
            type="profit"
            :msg="errorMessage"
        />
    </div>
</template>

<script>
    import Error from "./Error";

    export default {
        name: "InputLimit",
        components: {Error},
        props: {
            label: String,
            limits: Boolean,
            sumInv: Number,
            isValid: Boolean,
            needValidate: Number,
            detail: String,
        },
        data() {
            return{
                limit: this.isActive
                    ? this.limits ? 30 : Math.ceil(this.sumInv * 0.3)
                    : '',
                isActive: false,
                needError: false,
            }
        },
        computed: {
            currency: function() {
                return this.limits ? '%' : '$';
            },
            inputClass: function() {
                return this.isActive
                    ? !this.needError ?'limit-input-active':'limit-input-active limit-input-active-error'
                    : 'limit-input';
            },
            readonly: function() {
                return !this.isActive;
            },
            errorMessage: function() {
                if(this.detail === 'profit') {
                    return this.limits
                        ? 'Не может быть меньше 10%'
                        : `Не может быть меньше $ ${Math.ceil(this.sumInv * 0.1)}`
                } else if(this.detail === 'loss') {
                    if(this.limits) {
                        if (parseInt(this.limit) > 100) {
                            return 'Не может быть больше 100%';
                        } else if (parseInt(this.limit) < 10) {
                            return 'Не может быть меньше 10%';
                        }
                    } else {
                        if (parseInt(this.limit) > this.sumInv) {
                            return `Не может быть больше $ ${this.sumInv}`;
                        } else if (parseInt(this.limit) < Math.ceil(this.sumInv * 0.1)) {
                            return `Не может быть меньше $ ${Math.ceil(this.sumInv * 0.1)}`
                        }
                    }
                }
                return '';
            },
        },
        watch: {
            limits: function() {
                this.limit = this.isActive
                    ? this.limits ? 30 : Math.ceil(this.sumInv * 0.3)
                    : '';
            },
            isActive: function() {
                this.limit = this.isActive
                    ? this.limits ? 30 : Math.ceil(this.sumInv * 0.3)
                    : '';
            },
            limit: function() {
                this.validate();
            },
            needValidate: function() {
                this.needError = !this.isValid;
            },
        },
        methods: {
            validationInput: function(event) {
                if ( !/\d/.test( event.key ) ) {
                    event.preventDefault();
                    return false;
                }
            },
            onFocus: function() {
                this.needError = false;
                this.validate();
            },
            changeLimit: function () {
                this.validate();
            },
            validate: function() {
                let limitValid = false;
                if(this.detail === 'profit') {
                    if(this.limits) {
                        if (parseInt(this.limit) >= 10) {
                            limitValid = true;
                        }
                    } else {
                        if (parseInt(this.limit) >= Math.ceil(this.sumInv * 0.1)) {
                            limitValid = true;
                        }
                    }
                } else if(this.detail === 'loss') {
                    if(this.limits) {
                        if (parseInt(this.limit) >= 10 && parseInt(this.limit) <= 100) {
                            limitValid = true;
                        }
                    } else {
                        if (parseInt(this.limit) <= this.sumInv && parseInt(this.limit) >= Math.ceil(this.sumInv * 0.1)) {
                            limitValid = true;
                        }
                    }

                }
                this.$emit('input', {
                    data: this.isActive ? parseInt(this.limit) : -1,
                    isValid: this.isActive ? limitValid : true,
                });
            },
        },
    }
</script>

<style scoped>
    .limit {
        display: flex;
        align-items: center;
        height: 24px;
        margin-top: 9px;
        padding-left: 29px;
    }

    .limit-checkbox {
        width: 16px;
        height: 17px;
    }

    .limit-checkbox-label {
        margin-left: 8px;
        width: 98px;
        display: flex;
        align-items: center;
    }

    .limit-currency {
        position: absolute;
        margin-left: 145px;
        font-size: 13px;
        font-family: "Arial";
        color: rgb(144, 146, 148);
        line-height: 22px;
    }

    .limit-input {
        width: 126px;
        height: 24px;
        border: 1px solid rgb(192, 194, 196);
        border-radius: 3px;
        opacity: 0.502;
        text-align: end;
        font-size: 13px;
        font-family: "Arial";
        color: rgb(36, 37, 38);
        line-height: 22px;
        box-shadow: inset 0px 1px 2px 0px rgba(0, 0, 0, 0.15);
    }

    .limit-input-active {
        width: 126px;
        height: 24px;
        border: 1px solid rgb(192, 194, 196);
        border-radius: 3px;
        text-align: end;
        font-size: 13px;
        font-family: "Arial";
        color: rgb(36, 37, 38);
        line-height: 22px;
        box-shadow: inset 0px 1px 2px 0px rgba(0, 0, 0, 0.15);
        position: relative;
    }

    .limit-input-active-error {
        border-color: rgb(230, 69, 69);
    }
</style>
