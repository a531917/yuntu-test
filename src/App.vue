<template>
  <!-- 输入框 -->
  <div class="input-group">
    <input v-for="(input,index) in inputs" :key="index" 
    class="input-box" 
    :class="{'active': activeIndex === index}"
    v-model="inputs[index]" 
    maxlength="1"
    readonly
    @click.prevent="showKeyboard(index)" />
  </div>
  <!-- 结果显示 -->
  <div class="result">{{ result }}</div>
  <!-- 确认按钮 -->
  <button class="confirm-btn" @click.prevent="onConfirm">确认</button>
  <!-- 虚拟键盘 -->
  <KeyBoard class="keyboard" 
    v-if="showkeyboard"
    :chars="activeChars"
    :activeIndex="activeIndex"
    :inputs="inputs"
    @input="onInput"
    @activeIndex="activeIndex = $event"
    @confirm="onConfirm"
  ></KeyBoard>
  <!-- 遮罩层 -->
  <div v-if="showkeyboard" class="mask" @click.prevent="closeKeyboard()"></div>
  <!-- 提示信息 -->
  <div v-if="showToast" class="toast">{{ toastMessage }}</div>
</template>

<script>
import KeyBoard from './components/KeyBoard.vue';

export default {
  components: {
    KeyBoard
  },
  data() {
    return {
      showkeyboard: false,
      inputs: Array(6).fill(''), //输入框数组
      activeIndex: null, //当前激活输入框
      result: '', //结果
      toastMessage: '', //提示信息
      showToast: false //提示框是否可见
    }
  },
  computed: {
    activeChars() {
      if (this.activeIndex === 0) return ['我', '你', '他'];
      if (this.activeIndex === 1) return ['A', 'B', 'C','D','E','F','G'];
      return ['A', 'B', 'C','D','E','F','G','1', '2', '3','4','5','6','7','8','9','10'];
    }
  },
  methods: {
    showKeyboard(index) {
      // 聚焦到第一个空值
      this.activeIndex = this.inputs.findIndex(item => item === '');
      // 如果没有空值，则聚焦到点击的输入框
      if (this.activeIndex === -1) {
        this.activeIndex = index;
      }
      // 显示键盘
      this.showkeyboard = true;
    },
    closeKeyboard() {
      // 关闭键盘
      this.showkeyboard = false;
      this.activeIndex = null;
    },
    onInput(newInputs) {
      this.inputs = newInputs;
    },
    onConfirm() {
      // 获取非空的值
      const inputResult = this.inputs.filter(item => item !== '' );
      // 初始化结果
      this.result = '';
      // 值不足时弹出提示
      if (inputResult.length < 5){
        this.showMessage('请输入有效信息');
      }else{
        // 显示结果
        this.result = inputResult.join('');
      }
    },
    showMessage(msg) {
      this.toastMessage = msg;
      // 显示提示信息
      this.showToast = true;
      // 2秒后隐藏提示信息
      setTimeout(() => {
        this.showToast = false;
      }, 2000);
    }
  }
}
</script>

<style>
  .input-group{
    display: flex;
    justify-content: space-around;
    margin: 0 20px;
    margin-top: 100px;
  }
  .input-box{
    width: 32px;
    height: 32px;
    font-size: 25px;
    border-radius: 5px;
    border: 1px solid black;
    text-align: center;
    z-index: 1;
    caret-color: transparent;
    outline: none;
  }
  .input-box.active{
    border: 1px solid blue;
    outline: 1px solid blue;
    caret-color: black
  }
  .confirm-btn{
    width: 100px;
    height: 50px;
    margin-top: 200px;
    border-radius: 15px;
    margin-left: 50%;
    transform: translateX(-50%);
    background-color: white;
  }
  .keyboard{
    z-index: 1;
  }
  .mask{
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 0;
  }
  .result{
    margin-top: 50px;
    text-align: center;
    font-size: 30px;
  }
  .toast{
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: rgba(0, 0, 0, 0.7);
    color: white;
    padding: 15px 10px;
    border-radius: 5px;
    z-index: 2;
  }
</style>
