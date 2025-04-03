<template>
    <div class="keyboard">
        <!-- 可选字符 -->
        <div class="optional-char"> 
            <button v-for="(char,index) in chars" :key="index" 
            @click.prevent="onInput(char)"
            class="char-button" >
                {{ char }}
            </button>
        </div>
        <!-- 操作按钮 -->
        <div class="action-button">
            <button class="delete-button" :disabled="isdeletedisabled" @click="onDelete">删除</button>
            <button class="confirm-button" @click="onConfirm">确认</button>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                isdeletedisabled: false,
            }
        },
        props: {
            chars: Array,
            activeIndex: Number,
            inputs: Array,
        },
        onshow() {
            if (this.activeIndex === -1){
                this.isdeletedisabled = false;
            } else {
                this.isdeletedisabled = true;
            }
        },
        methods: {
            onInput(char) {
                // 拷贝父组件传过来的值
                const newInputs = [...this.inputs];
                let newActiveIndex = this.activeIndex;
                // 输入字符
                newInputs[newActiveIndex] = char;
                // 移动到下一个输入框
                if(newActiveIndex < 5) {newActiveIndex++;}
                // 更新父组件的值
                this.$emit('input',newInputs);
                this.$emit('activeIndex',newActiveIndex);
            },
            onDelete() {
                // 拷贝父组件传过来的值
                const newInputs = [...this.inputs];
                let newActiveIndex = this.activeIndex;
                // 如果当前输入框有值，则删除,并移动到上一个输入框
                if (newInputs[newActiveIndex] !== '') {
                    newInputs[newActiveIndex] = '';
                    if (newActiveIndex > 0) {
                        newActiveIndex--;
                    }
                } 
                // 如果当前输入框没有值，则移动到上一个输入框
                else if (newActiveIndex > 0) {
                    newActiveIndex--;
                }
                // 更新父组件的值
                this.$emit('input',newInputs);
                this.$emit('activeIndex',newActiveIndex);
            },
            onConfirm(){
                this.$emit('confirm')
            },
        }
    }
</script>

<style>
.keyboard {
    position: fixed;
    top: 70vh;
    bottom: 0;
    left: 0;
    right: 0;
    border: 1px solid black;
    background-color: #f5f5f5;
    padding: 20px;
}
.optional-char{
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    gap: 5px;
    padding: 10px;
}
.action-button{
    position: absolute;
    bottom: 20px;
    right: 20px;
}
.char-button{
    padding: 0;
    width: 40px;
    height: 40px;
    font-size: 20px;
    text-align: center;
}
.action-button>button{
    padding: 0;
    width: 64px;
    height: 32px;
    text-align: center;
    margin-left: 20px;
}
</style>