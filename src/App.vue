<template>
    <div class="section">
      <div class="inputField">{{ currentValue }}</div>
      <div class="buttons">
        <div @click="clear" class="btn small">AC</div>
        <div @click="sign" class="btn small">+/-</div>
        <div @click="percent" class="btn small">%</div>
        <div @click="backspace" class="btn small">Back</div>
        
        <div @click="append('7')" class="btn">7</div>
        <div @click="append('8')" class="btn">8</div>
        <div @click="append('9')" class="btn">9</div>
        <div @click="divide" class="btn operator">/</div>
       
        <div @click="append('4')" class="btn">4</div>
        <div @click="append('5')" class="btn">5</div>
        <div @click="append('6')" class="btn">6</div>
        <div @click="times" class="btn operator">x</div>
        
        <div @click="append('1')" class="btn">1</div>
        <div @click="append('2')" class="btn">2</div>
        <div @click="append('3')" class="btn">3</div>
        <div @click="minus" class="btn operator">-</div>
        
        <div @click="dot" class="btn">.</div>
        <div @click="append('0')" class="btn">0</div>
        <div @click="equals" class="btn">=</div>
        <div @click="plus" class="btn operator">+</div>
      </div>
    </div>
</template>

<script>

export default {
  data(){
    return{
      currentValue: '0',
      previousValue: null,
      operatorClicked: false,
      operator: null,
      digits: null
    }
  },
  methods: {
    clear(){
      this.currentValue = '0';
    },
    sign(){
      this.currentValue = `${this.currentValue.charAt(0)}` === '-' ? `${this.currentValue.slice(1)}` : `-${this.currentValue}`;
    },
    percent(){
      this.currentValue = parseFloat(this.currentValue) / 100;
    },
    backspace(){
      this.digits = this.currentValue.length;
      this.currentValue = this.currentValue.substring(0,this.digits-1);
    },
    setPrevious(){
      this.previousValue = this.currentValue;
      this.operatorClicked = true;
    },
    append(number){
      if(this.operatorClicked){
        this.currentValue = '';
        this.operatorClicked = false
      }
      if(this.currentValue.charAt(0) === '0'){
        this.currentValue = this.currentValue.slice(1);
      }
      this.currentValue = `${this.currentValue}`+`${number}`;
    },
    dot(){
      if(this.currentValue.indexOf('.') === -1){
        this.append('.');
      }
    },
    divide(){
      this.operator = (a, b) => a / b;
      this.setPrevious();
    },
    times(){
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },
    minus(){
      this.operator = (a, b) => a - b;
      this.setPrevious();
    },
    plus(){
      this.operator = (a, b) => a + b;
      this.setPrevious();   
    },
    equals(){
      if(this.previousValue){
        this.currentValue = `${this.operator(parseFloat(this.previousValue), parseFloat(this.currentValue))}`;
        this.currentValue = this.currentValue.slice(0,16);
        this.previousValue = null;
      }
      else{
        this.currentValue = this.currentValue.slice(0,16);
      }    
    }
  }
}
</script>
<style scoped>
  .section{
    margin: 0 auto;
    width: 250px;
    height: 320px;
    background-color: grey;
    border: 1px solid grey;
    border-radius: 5px;
    padding: 10px;
    box-shadow: 10px 10px 3px rgba(0, 0, 0, 0.19);  
  }
  .inputField{
    width: 240px;
    height: 40px;
    background-color: rgb(209, 207, 207);
    border-radius: 10px;
    margin-top: 5px;
    padding: 5px;
    text-align: right;
    font-size: 30px;
    overflow: hidden;
    margin-bottom: 20px;
  }
  .buttons{
    cursor: pointer;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(45px, auto);
    gap: 5px;
  }
  .btn{
    padding-top: 7px;
    border: 1px solid black;
    border-radius: 7px;
    text-align: center;
    font-size: 25px;
    font-weight: bold;
    background-color: rgb(207, 203, 203);
    color: black;
  }
  .small{
    border: 1px solid blue;
    font-size: 20px;
    background-color: rgb(121, 121, 233);
    color: white;
  }
  .operator{
    background-color: orange;
    color: white;
  }
</style>

