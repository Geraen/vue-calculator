<template>
    <div class="calculator">
        <div class="screen">{{current || 0}}</div>
        <div class="button clear" @click="clear">
            <span>C</span>
        </div>
        <div class="button" @click="sign">
            <span>&plusmn;</span>
        </div>
        <div class="button" @click="percent">
            <span>%</span>
        </div>
        <div class="button operator" @click="multiply">
            <span>*</span>
        </div>
        <div class="button number" @click="append('7')">
            <span>7</span>
        </div>
        <div class="button number" @click="append('8')">
            <span>8</span>
        </div>
        <div class="button number" @click="append('9')">
            <span>9</span>
        </div>
        <div class="button operator" @click="divide">
            <span>/</span>
        </div>
        <div class="button number" @click="append('4')">
            <span>4</span>
        </div>
        <div class="button number" @click="append('5')">
            <span>5</span>
        </div>
        <div class="button number" @click="append('6')">
            <span>6</span>
        </div>
        <div class="button operator" @click="minus">
            <span>-</span>
        </div>
        <div class="button number" @click="append('1')">
            <span>1</span>
        </div>
        <div class="button number" @click="append('2')">
            <span>2</span>
        </div>
        <div class="button number" @click="append('3')">
            <span>3</span>
        </div>
        <div class="button operator" @click="add">
            <span>+</span>
        </div>
        <div class="button number wide" @click="append('0')">
            <span>0</span>
        </div>
        <div class="button number" @click="dot">
            <span>.</span>
        </div>
        <div class="button result" @click="equal">
            <span>=</span>
        </div>
    </div>
</template>

<script lang="ts">
    import {Component, Emit, Vue} from 'vue-property-decorator';

    @Component
    export default class Calculator extends Vue {
        current: string = '';
        previous!: any;
        operator!: any;
        operatorClicked!: boolean;

        data() {
            return {};
        }

        @Emit() clear() {
            this.current = '';
        }

        @Emit() sign() {
            if (this.current !== '0') {
                if (this.current.charAt(0) !== '-') {
                    this.current = '-' + this.current;
                } else {
                    this.current = this.current.slice(1);
                }
            }
        }

        @Emit() percent() {
            this.current = +this.current / 100;
        }

        @Emit() append(n: string) {
            if (this.operatorClicked) {
                this.current = '';
                this.operatorClicked = false;
            }
            this.current += n;
        }

        @Emit() dot() {
            if (this.current.indexOf('.') === -1) {
                this.append('.');
            }
        }

        @Emit() setPrevious() {
            this.previous = this.current;
            this.operatorClicked = true;
        }

        @Emit() multiply() {
            this.operator = (a, b) => a * b;
            this.setPrevious();
        }

        @Emit() divide() {
            this.operator = (a, b) => a / b;
            this.setPrevious();
        }

        @Emit() minus() {
            this.operator = (a, b) => a - b;
            this.setPrevious();
        }

        @Emit() add() {
            this.operator = (a, b) => a + b;
            this.setPrevious();
        }

        @Emit() equal() {
            this.current = this.operator(parseFloat(this.previous), parseFloat(this.current)).toString();
            this.previous = null;
        }
    }
</script>

<style scoped>
    .calculator,
    .button,
    .screen {
        border-radius: 10px;
    }

    .calculator,
    .screen {
        padding: 10px;
    }

    .calculator {
        background-color: #aaa;
        width: 25%;
        height: auto;
        margin: 15% auto 0;
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-auto-rows: minmax(50px, auto);
    }

    .screen {
        grid-column: 1/5;
        /*height: 100px;*/
        background-color: rgba(0, 85, 205, 0.55);
        color: #fff;
        margin-bottom: 5px;
        text-align: right;
        font-size: 24px;
        font-weight: bold;
    }

    .button {
        background-color: #ddd;
        align-content: stretch;
        margin: 2px;
    }

    .button:hover {
        cursor: pointer;
        background-color: #ccc;
    }

    .button > span {
        align-self: center;
        line-height: 50px;
    }

    .button.number {
        background-color: #fff;
    }

    .button.number:hover {
        background-color: #eee;
    }

    .button.operator,
    .button.result {
        background-color: rgba(255, 131, 0, 0.5);
    }

    .button.operator:hover,
    .button.result:hover {
        background-color: rgba(255, 131, 0, 0.75);;
    }

    .button.number.wide {
        grid-column: 1/3;
    }
</style>
