<template>
    <div class="panel">
        <h2>Введите пин-код</h2>
        <code-input :visible="show" @input-code="inputFF"></code-input>
        <p class="example-output">Ввдееный код: {{ valueCode }}</p>
        <div class="button-wrapper">
            <a class="btn btn-cusses" href="#" @click.prevent="showInput">{{ show ? 'Скрыть ввод' : 'Показать ввод'}}</a>
        </div>
    </div>
</template>

<script>
import CodeInput from './components/CodeInputComponent/CodeInputCss';

export default {
    name: 'App',
    components: {
        CodeInput
    },
    data() {
        return {
            show: true,
            valueCode: ''
        }
    },
    methods: {
        showInput() {
            this.show = !this.show;
        },
        inputFF(e) {
            this.valueCode = e.value;
        }
    }
}
</script>

<style lang="sass">
@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@300;400&display=swap')
@import "assets/style/style"
</style>
