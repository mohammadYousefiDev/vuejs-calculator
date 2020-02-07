<template>
  <div>

    <div class="calculator">
      <span class="display bordered-top">{{result}}</span>
      <span @click="returned" class="returned operator">C</span>
      <span @click="addMinus" class="operator">-/+</span>
      <span @click="remain" class="operator">%</span>
      <span @click="divide" class="operator">/</span>
      <span @click="append('7')">7</span>
      <span @click="append('8')">8</span>
      <span @click="append('9')">9</span>
      <span @click="times" class="operator">*</span>
      <span @click="append('4')">4</span>
      <span @click="append('5')">5</span>
      <span @click="append('6')">6</span>
      <span @click="minus" class="operator">-</span>
      <span @click="append('1')">1</span>
      <span @click="append('2')">2</span>
      <span @click="append('3')">3</span>
      <span @click="add" class="operator">+</span>
      <span class="zero bordered-b-l" @click="append('0')">0</span>
      <span @click="dot">.</span>
      <span @click="equal" class="operator bordered-b-r">=</span>
    </div>

    <div class="logs">
      <ul>
        <li v-for="(currentLog, index) in currentLogs" :key="index">
          {{log(currentLog.current, currentLog.operator, currentLog.previous)}}
        </li>
      </ul>
    </div>
  </div>
  
</template>

<script>
export default {
  data() {
    return {
      result: '0',
      previous: null,
      operator: '',
      currentOperator: '',
      operatorClicked: false,
      new: false,
      currentLogs: []
    }
  },
  methods: {
    append: function(item) {
      if( this.new ){
        this.result = '';
        this.new = false
      }
      if ( this.operatorClicked ) {
        this.result = '';
        this.operatorClicked = false;
      }
      if ( this.result == 0) {
        this.result = '' + item;
      } else {
        this.result = this.result + item;
      }
      
    },
    returned: function() {
      this.result = 0;
    },
    dot: function() {
      if ( this.result.indexOf('.') === -1 ) {
        this.append('.');
      }
    },
    addMinus: function(){
      this.result *= -1
    },
    setPrevius: function() {
      this.previous = this.result;
      this.operatorClicked = true;
    },
    divide: function() {
      this.operator = (a, b) => a/b;
      this.setPrevius();
      this.currentOperator = '/';
    },
    remain: function() {
      this.operator = (a, b) => a%b;
      this.setPrevius();
      this.currentOperator = '%';
    },
    times: function() {
      this.operator = (a, b) => a*b;
      this.setPrevius();
      this.currentOperator = '*';
    },
    minus: function() {
      this.operator = (a, b) => a-b;
      this.setPrevius();
      this.currentOperator = '-';
    },
    add: function() {
      this.operator = (a, b) => a+b;
      this.setPrevius();
      this.currentOperator = '+';
    },
    equal: function() {
      this.currentLogs.push(
        {
          previous: this.previous,
          operator: this.currentOperator,
          current: this.result
        }
      );

      this.result = this.operator(
        parseFloat(this.previous), 
        parseFloat(this.result)
      );
      this.previous = null;
      this.new = true;
    },
    log(a,b,c){
      return c + ' ' + b + ' ' + a + ' = ' + eval(c + ' ' + b + ' ' + a)
    }
  }
};
</script>

<style scoped lang="scss">
.logs{
  width: 300px;
  margin: 20px auto 0;
  height: 150px;
  background: #eee;
  border-radius: 5px;
  overflow-y: scroll;
}
ul{
  padding-left: 10px;
  text-align: left;
  line-height: 30px;
  font-size: 14px;
  list-style: none;
}
.calculator {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  width: 300px;
  margin: 0 auto;
  background: #fff;
  box-shadow: 0 3px 10px #ddd;
  color: #000;

  span {
    height: 55px;
    line-height: 55px;
    border: 1px solid #ddd;
    cursor: pointer;
    transition: 0.05s;
    font-size: 20px;

    &:hover {
      background: #f2f2f2;
      transition: 0.05s;
    }
  }
}
.bordered-top{
    border-top-left-radius: 5px;
    border-top-right-radius: 5px;
}
.bordered-b-r{
    border-bottom-right-radius: 5px;
}
.bordered-b-l{
    border-bottom-left-radius: 5px;
}
.display {
  grid-column: 1/5;
  background: rgba(97, 0, 187, 0.822);
  font-size: 40px !important;
  color: #fff;

  &:hover {
    background-color: rgba(97, 0, 187, 0.822) !important;
    cursor: auto !important;
  }
}
.zero {
  grid-column: 1/3;
}
.operator {
  background: orange;
  color: #fff;
  border: 0;
  &:hover {
      background: #ff6b27 !important;
  }
}
</style>
