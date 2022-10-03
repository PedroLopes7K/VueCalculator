<template>
  <div class="calculator">
    <div class="display">{{current || '0'}}</div>
    <div @click="clear" class="btn">C</div>
    <div @click="sign" class="btn">+/-</div>
    <div @click="percent" class="btn">%</div>
    <div @click="divide('/')" class="btn operator">÷</div>
    <div @click="append('7')" class="btn">7</div>
    <div @click="append('8')" class="btn">8</div>
    <div @click="append('9')" class="btn">9</div>
    <div @click="times('x')" class="btn operator">x</div>
    <div @click="append('4')" class="btn">4</div>
    <div @click="append('5')" class="btn">5</div>
    <div @click="append('6')" class="btn">6</div>
    <div @click="minus('-')" class="btn operator">-</div>
    <div @click="append('1')" class="btn">1</div>
    <div @click="append('2')" class="btn">2</div>
    <div @click="append('3')" class="btn">3</div>
    <div @click="add('+')" class="btn operator">+</div>
    <div @click="append('0')" class="btn zero">0</div>
    <div @click="dot" class="btn">.</div>
    <div @click="equal" class="btn operator">=</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      previous: null,
      middle: null,
      current: '',
      currentnumber: '',
      currentOperator: null,
      operator: null,
      symbolOperator: '',
      countOpetators: 0,
      countClickOperator: 0,
      countClickOperatorValidate: 1,
      operatorClicked: false,
      testeControl: true,
     control: false
    }
  },
  methods: {
    clear() {
      this.previous= null,
      this.middle = null,
      this.current = '',
      this.currentnumber =  '',
      this.currentOperator = null,
      this.operator = null,
      this.symbolOperator = '',
      this.countOpetators = 0,
      this.countClickOperator = 0,
      this.countClickOperatorValidate = 1,
      this.operatorClicked = false,
      this.testeControl = true,
      this.control = false
 
    },
    sign() {
      this.current =
        this.current.charAt(0) === '-'
          ? this.current.slice(1)
          : `-${this.current}`
    },
    percent() {
      this.current = `${parseFloat(this.current) / 100}`
    },
    append(number) {
      // APENAS FALTANDO RESOLVER PROBLEMA DO DOT
      if(this.countClickOperatorValidate > 1 && this.countClickOperator == this.countClickOperatorValidate){
        this.currentnumber = `${this.currentnumber}${number}`
      }
    
      if(this.countOpetators >= 2){
        this.middle = this.current
        console.log(this.currentnumber)
        this.current = `${this.currentOperator(
        parseFloat(this.previous),
        parseFloat(this.currentnumber)
      )}`
      this.previous = this.current
      this.countOpetators = 1
      this.currentnumber = ''
      }


      if (this.operatorClicked) {
        this.currentOperator = this.operator
        this.currentnumber = `${this.currentnumber}${number}`
  
        if(this.testeControl){
        
        this.current = this.current + this.symbolOperator
        
         this.testeControl = false
        }

        if(this.countClickOperatorValidate < this.countClickOperator){
         this.current = this.current + this.symbolOperator
         this.countClickOperatorValidate = this.countClickOperator
         this.operatorClicked = false

        }
      }

    this.current = `${this.current}${number}`
    },

    dot() {
      if (this.current.indexOf('.') === -1) {
        this.append('.')
      }
    },
    setPrevious() {
      this.previous = this.current
      this.operatorClicked = true
      this.countClickOperator = this.countClickOperator + 1
    },
    divide(symbol) {
      this.symbolOperator = symbol
      this.countOpetators++
      this.operator = (a, b) => a / b
      this.setPrevious()

    },
    times(symbol) {
      this.symbolOperator = symbol
      this.countOpetators++
      this.operator = (a, b) => a * b
      this.setPrevious()

    },
    minus(symbol) {
      this.symbolOperator = symbol
      this.countOpetators++
      this.operator = (a, b) => a - b
      this.setPrevious()

    },
    add(symbol) {
      this.symbolOperator = symbol
      this.countOpetators++
      this.operator = (a, b) => a + b
      this.setPrevious()
    },
    equal() {
      console.log(this.previous)
      console.log(this.currentnumber)
      this.countOpetators = 0
      this.current = `${this.operator(
        parseFloat(this.previous),
        parseFloat(this.currentnumber)
      )}`
      this.previous = ''
      this.currentnumber = ''
      this.countClickOperator = 0
      this.countClickOperatorValidate = 1
      this.operatorClicked  = false
      this.testeControl  = true
    }
  }
}
</script>

<style scoped>
.calculator {
  margin: 0 auto;
  width: 400px;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}

.display {
  grid-column: 1 / 5;
  background-color: #333;
  color: white;
}

.zero {
  grid-column: 1 / 3;
}

.btn {
  background-color: #f2f2f2;
  border: 1px solid #999;
}

.operator {
  background-color: orange;
  color: white;
}
</style>
