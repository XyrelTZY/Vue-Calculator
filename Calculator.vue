<template>
  <div>
    <button @click="ON" v-if="!isVisible" class="w-20 h-20 bg-green-500 text- rounded-lg font-semibold ml-[50rem] mt-[11rem]">ON</button>
    <div v-if="isVisible" class="flex items-center justify-center" id="display">
      <div class="w-[21.2rem] mt-24 bg-gray-600 rounded-lg">
        <div class="outline-none bg-transparent border-b border-gray-500 border-t mt-2 w-[21rem] h-16 mb-3 font-bold text-white pr-2 flex items-center justify-end overflow-hidden">
          {{ displayValue }}
        </div>
        <ul class="flex flex-row my-1 mx-1 gap-1">
          <li><button @click="clear" class="w-20 h-20 bg-red-600 active:scale-[0.98] rounded-sm">C</button></li>
          <li><button @click="setOperation('*')" class="w-20 h-20 bg-yellow-300 active:scale-[0.98] rounded-sm">*</button></li>
          <li><button @click="setOperation('/')" class="w-20 h-20 bg-yellow-300 active:scale-[0.98] rounded-sm">/</button></li>
          <li><button @click="OFF" class="w-20 h-20 bg-yellow-300 active:scale-[0.98] rounded-sm font-bold">OFF</button></li>
        </ul>
        <ul class="flex flex-row my-1 mx-1 gap-1">
          <li><button @click="appendNumber('1')" class="w-20 h-20 bg-gray-400 active:scale-[0.98] rounded-sm font-bold">1</button></li>
          <li><button @click="appendNumber('2')" class="w-20 h-20 bg-gray-400 active:scale-[0.98] rounded-sm font-bold">2</button></li>
          <li><button @click="appendNumber('3')" class="w-20 h-20 bg-gray-400 active:scale-[0.98] rounded-sm font-bold">3</button></li>
          <li><button @click="setOperation('-')" class="w-20 h-20 bg-yellow-300 active:scale-[0.98] rounded-sm">-</button></li>
        </ul>
        <ul class="flex flex-row my-1 mx-1 gap-1">
          <li><button @click="appendNumber('4')" class="w-20 h-20 bg-gray-400 active:scale-[0.98] rounded-sm font-bold">4</button></li>
          <li><button @click="appendNumber('5')" class="w-20 h-20 bg-gray-400 active:scale-[0.98] rounded-sm font-bold">5</button></li>
          <li><button @click="appendNumber('6')" class="w-20 h-20 bg-gray-400 active:scale-[0.98] rounded-sm font-bold">6</button></li>
          <li><button @click="setOperation('+')" class="w-20 h-20 bg-yellow-300 active:scale-[0.98] rounded-sm">+</button></li>
        </ul>
        <ul class="flex flex-row my-1 mx-1 gap-1">
          <li><button @click="appendNumber('7')" class="w-20 h-20 bg-gray-400 active:scale-[0.98] rounded-sm font-bold">7</button></li>
          <li><button @click="appendNumber('8')" class="w-20 h-20 bg-gray-400 active:scale-[0.98] rounded-sm font-bold">8</button></li>
          <li><button @click="appendNumber('9')" class="w-20 h-20 bg-gray-400 active:scale-[0.98] rounded-sm font-bold">9</button></li>
          <li><button @click="calculateResult" class="w-20 h-20 bg-yellow-300 active:scale-[0.98] rounded-sm">=</button></li>
        </ul>
      </div>
    </div>
    <div v-if="showingVideo" class="video-container">
      <video src="../assets/video/Video.mp4" autoplay @ended="hideVideo" class="video"></video>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      displayValue: '',
      currentOperation: null,
      operand1: null,
      isVisible: true,
      showingVideo: false, 
      plusOperationUsed: false,
    };
  },
  methods: {
    appendNumber(number) {
      this.displayValue += number;
    },
    setOperation(operation) {
      if (this.currentOperation) {
        this.calculateResult();
      }
      this.operand1 = parseFloat(this.displayValue) || 0; 
      this.currentOperation = operation;
      this.displayValue = ''; 
      this.plusOperationUsed = (operation === '+');
    },
    clear() {
      this.displayValue = '';
      this.currentOperation = null;
      this.operand1 = null;
      this.showingVideo = false; 
      this.plusOperationUsed = false; 
    },
    calculateResult() {
      if (this.currentOperation && this.operand1 !== null) {
        const operand2 = parseFloat(this.displayValue);
        let result;
        switch (this.currentOperation) {
          case '+':
            result = "I MISS YOUUUUUUUUUUUUUUUUUUUUUUUUU";
            break;
          case '-':
            result = this.operand1 - operand2;
            break;
          case '*':
            result = this.operand1 * operand2;
            break;
          case '/':
            result = operand2 !== 0 ? this.operand1 / operand2 : 'Error'; 
            break;
          default:
            result = 'Error';
        }
        this.displayValue = result.toString();
        this.currentOperation = null;
        this.operand1 = null;
        if (this.plusOperationUsed) {
          this.showVideo();
        }
        this.plusOperationUsed = false; 
      }
    },
    showVideo() {
      this.showingVideo = true;
    },
    hideVideo() {
      this.showingVideo = false; 
    },
    OFF(){
      this.isVisible = true;
    },
    ON (){
      this.isVisible = false;
    }
  },
};
</script>

<style scoped>
.video-container {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1000;
}

.video {
  width: 500px;
  height: 300px;
  object-fit: cover;
  border: 2px solid black; 
}
</style>
