<template>
    <div class="code-input-wrapper">
        <div class="input-item" v-for="(item, index) of count" :key="item">
            <input
                    autocapitalize="off"
                    :data-id="item"
                    :maxlength="maxlength"
                    :class="{'not-empty' : codeValue[index]}"
                    :type="typeInput"
                    v-model="codeValue[index]"
                    @paste.prevent="pasteEvent"
                    @keydown="deleteEvent"
                    @input="inputEvent">
            <div v-if="codeValue[index] && !visible" class="code-input-hidden" @click.prevent="focusEvent">
                <img :src="hideIcon" alt="">
            </div>
        </div>
    </div>
</template>

<script>
    import hideIcon from "./hide_icon.svg";

    export default {
        name: "CodeInput",
        props: {
            count: {
                type: Number,
                default: 5
            },
            maxlength: {
                type: Number,
                default: 1
            },
            visible: {
                type: Boolean,
                default: true
            },
            type: {
                type: String,
                default: 'text'
            }
        },
        computed: {
            typeInput() {
                return this.visible ? 'text' : 'password'
            }
        },
        data() {
            return {
                codeValue: [],
                hideIcon: hideIcon
            }
        },
        methods: {
            inputEvent(e) {
                const item = Number.parseInt(e.target.dataset.id);

                if (this.type === 'number' && !parseInt(e.data)) {
                    e.target.value = '';
                    return false;
                }

                if (e.data && (item + 1) <= this.count) {
                    document.querySelector('[data-id="' + (item + 1) + '"]').focus();
                }

                this.sendEmitObject(e);
            },
            pasteEvent(e) {
                let paste = e.clipboardData.getData('text/plain').split('');
                let length = 0;

                if (this.type === 'number') {
                    paste = paste.filter(item => parseInt(item));
                }

                paste = paste.splice(0, this.count);
                this.codeValue = paste;

                length = paste.length === this.count ? paste.length : paste.length + 1;
                document.querySelector('[data-id="' + length + '"]').focus();

                this.sendEmitObject(e);
            },
            deleteEvent(e) {
                const target = e.target;
                const item = Number.parseInt(e.target.dataset.id);

                if (e.code === 'Backspace' &&  (item - 1) > 0 && !target.value) {
                    document.querySelector('[data-id="' + (item - 1) + '"]').focus();
                }
            },
            focusEvent(e) {
                const currentElement = e.currentTarget ? e.currentTarget : e.target;
                const previewInput = currentElement.previousElementSibling;
                previewInput.focus();
                const value = previewInput.value;
                previewInput.value = '';
                previewInput.value = value;
            },
            sendEmitObject(e) {
                this.$emit('inputCode', {
                    value: this.codeValue.join(''),
                    target: e,
                    targetCurrent: document.querySelector('.code-input-wrapper')
                });
            }
        }
    }
</script>

<style>

    .code-input-wrapper{
        display: flex;
    }

    .code-input-wrapper input{
        outline: none;
        border: 1px solid #e3e6e7;
        border-radius: 0.25rem;
        font-family: 'Raleway', sans-serif;
        font-size: 1.5rem;
        padding: 0.5rem;
        width: 30px;
        text-align: center;
    }

    .code-input-wrapper input:focus{
        border-color: #515861;
    }

    .code-input-wrapper input.not-empty{
        border-color: #6acda3;
    }

    .code-input-wrapper .input-item{
        position: relative;
        margin: 0 0.5rem;
    }

    .code-input-wrapper .code-input-hidden{
        position: absolute;
        background-color: #fff;
        top: 2px;
        bottom: 2px;
        left: 2px;
        right: 2px;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .code-input-wrapper .code-input-hidden img{
        width: 30%;
        height: auto;
    }

</style>