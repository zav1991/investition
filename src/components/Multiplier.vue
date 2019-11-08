<template>
    <div class="multiplier">
        <div class="multiplier-label">{{label}}</div>
        <div class="multiplier-input-wrapper">
            <div class="multiplier-input-currency">x</div>
            <input
                :class="inputClass"
                type="text"
                v-model="multiplier"
                @keypress="validationInput"
                @input="changeMultiplier"
                onpaste="return false"
                @focus="onFocus"
                @focusin="openSlider"
                id="inputMult"
                v-click-outside="hideSlider"
            />
            <div
                v-if="isNeedSlider"
                class="slider-wrapper"
                id="sliderWrapper"
            >
                <input
                    class="input-slider"
                    type="range"
                    min="1"
                    max="40"
                    step="1"
                    v-model="multiplier"
                    @input="changeMultiplier"
                />
                <span class="slider-label label-one">1</span>
                <span class="slider-label label-five">5</span>
                <span class="slider-label label-twenty">20</span>
                <span class="slider-label label-forty">40</span>
            </div>
            <Error
                v-if="needError"
                type="mult"
                msg="Неверное значение мультипликатора"
            />
        </div>
    </div>
</template>

<script>
    import ClickOutside from 'vue-click-outside';

    import Error from "./Error";

    export default {
        name: "Multiplier",
        components: {Error},
        directives: {ClickOutside},
        props: {
            mult: Number,
            needValidate: Number,
            isValidMult: Boolean,
        },
        data() {
            return {
                label: 'Мультипликатор',
                multiplier: this.mult,
                needError: false,
                isNeedSlider: false,
            }
        },
        computed: {
            inputClass: function() {
                return !this.needError ? 'multiplier-input' : 'multiplier-input multiplier-input-error'
            },
        },

        watch: {
            needValidate: function() {
                this.needError = !this.isValidMult;
            },
        },
        methods: {
            hideSlider: function() {
                this.isNeedSlider = false;
            },
            openSlider: function() {
                this.isNeedSlider = true;
            },
            validationInput: function(event) {
                if ( !/\d/.test( event.key ) ) {
                    event.preventDefault();
                    return false;
                }
            },
            changeMultiplier: function() {
                const currentMult = parseInt(this.multiplier);
                if (currentMult < 0) {
                    this.multiplier = '0';
                } else if(currentMult > 99) {
                    this.multiplier = '99';
                } else if(this.multiplier === '') {
                    this.multiplier = '0';
                }
                this.validate();
            },
            onFocus: function() {
                this.needError = false;
                this.validate();
            },
            validate: function() {
                this.$emit('input', {
                    mult: parseInt(this.multiplier),
                    isValidMult: parseInt(this.multiplier) >= 1 && parseInt(this.multiplier) <= 40
                });
            },
        },
    }
</script>

<style scoped>
    .multiplier {
        display: flex;
        justify-content: space-between;
        margin-top: 9px;
        width: 208px;
    }

    .multiplier-label {
        width: calc(50% - 29px);
        font-family: 'Arial Regular';
        font-size: 13px;
        line-height: 22px;
        display: flex;
        padding-left: 29px;
        align-items: center;
    }

    .multiplier-input-wrapper {
        display: flex;
        align-items: center;
    }

    .multiplier-input-currency {
        position: absolute;
        margin-left: 8px;
        z-index: 1;
        font-family: "Arial Regular";
        font-size: 13px;
        line-height: 22px;
        color: rgb(144, 146, 148);
    }

    .multiplier-input {
        text-align: end;
        font-family: "Arial Regular";
        font-size: 13px;
        line-height: 22px;
        border: 1px solid rgb(192, 194, 196);
        border-radius: 3px;
        padding: 7px 8px;
        width: 32px;
        height: 10px;
        position: relative;
    }

    .multiplier-input-error {
        border-color: rgb(230, 69, 69);
    }

    .slider-label {
        font-size: 11px;
        font-family: "Arial";
        color: rgb(144, 146, 148);
        line-height: 22px;
        position: absolute;
        margin-top: 35px;
    }

    .label-one {
        margin-left: -108px;
    }

    .label-five {
        margin-left: -85px;
    }

    .label-twenty {
        margin-left: -2px;
    }

    .label-forty {
        margin-left: 108px;
    }

    .slider-wrapper {
        position: absolute;
        margin-left: -137px;
        margin-top: 52px;
        width: 280px;
        height: 61px;
        border-radius: 3px;
        background-color: #3a3b3c;
        z-index: 2;
        display: flex;
        /*align-items: center;*/
        justify-content: center;
    }

    .slider-wrapper:after {
        content: '';
        position: absolute;
        border: 6px solid transparent;
        border-bottom: 6px solid #3a3b3c;
        top: -12px;
        left: 160px;
        z-index: 3;
    }

    .input-slider {

    }

    input[type=range] {
        -webkit-appearance: none;
        margin: 7px 0;
        z-index: 2;
        background-color: transparent;
        width: 232px;
    }
    input[type=range]:focus {
        outline: none;
    }
    input[type=range]::-webkit-slider-runnable-track {
        width: 100%;
        height: 4px;
        cursor: pointer;
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0), 0px 0px 0px rgba(13, 13, 13, 0);
        background: #7e98e5;
        border-radius: 25px;
        border: 0px solid rgba(1, 1, 1, 0);
    }
    input[type=range]::-webkit-slider-thumb {
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0), 0px 0px 0px rgba(13, 13, 13, 0);
        border: 6px solid #7e98e5;
        height: 18px;
        width: 18px;
        border-radius: 10px;
        background: #ffffff;
        cursor: pointer;
        -webkit-appearance: none;
        margin-top: -7px;
    }
    input[type=range]:focus::-webkit-slider-runnable-track {
        background: #a0b3ec;
    }
    input[type=range]::-moz-range-track {
        width: 100%;
        height: 4px;
        cursor: pointer;
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0), 0px 0px 0px rgba(13, 13, 13, 0);
        background: #7e98e5;
        border-radius: 25px;
        border: 0px solid rgba(1, 1, 1, 0);
    }
    input[type=range]::-moz-range-thumb {
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0), 0px 0px 0px rgba(13, 13, 13, 0);
        border: 6px solid #7e98e5;
        height: 18px;
        width: 18px;
        border-radius: 10px;
        background: #ffffff;
        cursor: pointer;
    }
    input[type=range]::-ms-track {
        width: 100%;
        height: 4px;
        cursor: pointer;
        background: transparent;
        border-color: transparent;
        color: transparent;
    }
    input[type=range]::-ms-fill-lower {
        background: #5c7dde;
        border: 0px solid rgba(1, 1, 1, 0);
        border-radius: 50px;
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0), 0px 0px 0px rgba(13, 13, 13, 0);
    }
    input[type=range]::-ms-fill-upper {
        background: #7e98e5;
        border: 0px solid rgba(1, 1, 1, 0);
        border-radius: 50px;
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0), 0px 0px 0px rgba(13, 13, 13, 0);
    }
    input[type=range]::-ms-thumb {
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0), 0px 0px 0px rgba(13, 13, 13, 0);
        border: 6px solid #7e98e5;
        height: 18px;
        width: 18px;
        border-radius: 10px;
        background: #ffffff;
        cursor: pointer;
        height: 4px;
    }
    input[type=range]:focus::-ms-fill-lower {
        background: #7e98e5;
    }
    input[type=range]:focus::-ms-fill-upper {
        background: #a0b3ec;
    }

    /*input[type=range] {*/
    /*    height: 30px;*/
    /*    -webkit-appearance: none;*/
    /*    margin: 10px 0;*/
    /*    width: 100%;*/
    /*    background-color: transparent;*/
    /*}*/
    /*input[type=range]:focus {*/
    /*    outline: none;*/
    /*}*/
    /*input[type=range]::-webkit-slider-runnable-track {*/
    /*    width: 100%;*/
    /*    height: 4px;*/
    /*    cursor: pointer;*/
    /*    animate: 0.2s;*/
    /*    box-shadow: 0px 0px 0px #000000;*/
    /*    background: #7E98E5;*/
    /*    border-radius: 1px;*/
    /*    border: 0px solid #000000;*/
    /*}*/
    /*input[type=range]::-webkit-slider-thumb {*/
    /*    box-shadow: 0px 0px 0px #000000;*/
    /*    border: 6px solid #7E98E5;*/
    /*    height: 18px;*/
    /*    width: 18px;*/
    /*    border-radius: 17px;*/
    /*    background: #FFFFFF;*/
    /*    cursor: pointer;*/
    /*    -webkit-appearance: none;*/
    /*    margin-top: -10px;*/
    /*}*/
    /*input[type=range]:focus::-webkit-slider-runnable-track {*/
    /*    background: #7E98E5;*/
    /*}*/
    /*input[type=range]::-moz-range-track {*/
    /*    width: 100%;*/
    /*    height: 4px;*/
    /*    cursor: pointer;*/
    /*    animate: 0.2s;*/
    /*    box-shadow: 0px 0px 0px #000000;*/
    /*    background: #7E98E5;*/
    /*    border-radius: 1px;*/
    /*    border: 0px solid #000000;*/
    /*}*/
    /*input[type=range]::-moz-range-thumb {*/
    /*    box-shadow: 0px 0px 0px #000000;*/
    /*    border: 6px solid #7E98E5;*/
    /*    height: 18px;*/
    /*    width: 18px;*/
    /*    border-radius: 17px;*/
    /*    background: #FFFFFF;*/
    /*    cursor: pointer;*/
    /*}*/
    /*input[type=range]::-ms-track {*/
    /*    width: 100%;*/
    /*    height: 4px;*/
    /*    cursor: pointer;*/
    /*    animate: 0.2s;*/
    /*    background: transparent;*/
    /*    border-color: transparent;*/
    /*    color: transparent;*/
    /*}*/
    /*input[type=range]::-ms-fill-lower {*/
    /*    background: #7E98E5;*/
    /*    border: 0px solid #000000;*/
    /*    border-radius: 2px;*/
    /*    box-shadow: 0px 0px 0px #000000;*/
    /*}*/
    /*input[type=range]::-ms-fill-upper {*/
    /*    background: #7E98E5;*/
    /*    border: 0px solid #000000;*/
    /*    border-radius: 2px;*/
    /*    box-shadow: 0px 0px 0px #000000;*/
    /*}*/
    /*input[type=range]::-ms-thumb {*/
    /*    margin-top: 1px;*/
    /*    box-shadow: 0px 0px 0px #000000;*/
    /*    border: 6px solid #7E98E5;*/
    /*    height: 18px;*/
    /*    width: 18px;*/
    /*    border-radius: 17px;*/
    /*    background: #FFFFFF;*/
    /*    cursor: pointer;*/
    /*}*/
    /*input[type=range]:focus::-ms-fill-lower {*/
    /*    background: #7E98E5;*/
    /*}*/
    /*input[type=range]:focus::-ms-fill-upper {*/
    /*    background: #7E98E5;*/
    /*}*/
</style>