<template>
<div id="app">
  <div class="calculator">
    <div class="result">
      {{ output }}
    </div>

    <button class="btn_ac" @click="clear">AC</button>
    <button class="btn_pm" @click="calToggle">+/-</button>
    <button class="btn_del" @click="calDelete">DEL</button>
    <button class="btn_div" @click="append('÷')">÷</button>

    <button class="btn_num_7" @click="append(7)">7</button>
    <button class="btn_num_8" @click="append(8)">8</button>
    <button class="btn_num_9" @click="append(9)">9</button>
    <button class="btn_mul" @click="append('×')">×</button>
    
    <button class="btn_num_4" @click="append(4)">4</button>
    <button class="btn_num_5" @click="append(5)">5</button>
    <button class="btn_num_6" @click="append(6)">6</button>
    <button class="btn_sub" @click="append('-')">-</button>

    <button class="btn_num_1" @click="append(1)">1</button>
    <button class="btn_num_2" @click="append(2)">2</button>
    <button class="btn_num_3" @click="append(3)">3</button>
    <button class="btn_pls" @click="append('+')">+</button>    
    
    <button class="btn_num_0" @click="append(0)">0</button>
    <button class="btn_dot" @click="append('.')">.</button>
    <button class="btn_eq" @click="calculate">=</button>
  </div>
</div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      output :0,
      outflag: false,
      opflag: false,
      inputflag: false,
    }
  },
  mounted () {
    window.addEventListener("keypress", this.clickKey)
  },
  methods: {
    clear(){
      this.outflag = false;
      this.opflag= false;
      this.inputflag= false;
      this.output = 0;
    },
    isOper(character) {
      return ['+', '-', '×', '÷'].indexOf(character) > -1
    },
    append(character) {
          console.log("cc")
      if (this.output === '0' && !this.isOper(character)) {
        if (character === '.') {
          this.output += '' + character
          this.inputflag = true
        } else {
          this.output = '' + character
        }
        
        this.outflag = true
        return
      }
      
      // 숫자인경우
      if (!this.isOper(character)) {
        if (character === '.' && this.inputflag) {
          return
        }
        this.outflag = true
        if (character === '.') {
          this.inputflag = true
          this.opflag = true
        } else {
          this.opflag = false
        }
        if(this.output===0){
          if(character==='.'&& this.inputflag==true)
          {
            this.output = "0"+character
          }
          else{
            console.log("aaaaaaaaaaa")
            this.output = '' + character
          }
          
        }else{
          console.log("jjjjjjjj")
          if(character==='.'&& this.inputflag==true && this.isOper(this.output[this.output.length-1]))
          {
            console.log("nnnnnnnnnnnnn")
            this.output += "0"+character
          }
          else{
            this.output += '' + character
          }
          
        }
        
      }
      
      // 연산일경우 
      if (this.isOper(character) && !this.opflag) {
        this.output += '' + character
        this.inputflag = false
        this.opflag = true
      }
    },
    calDelete(){
      if(this.outflag == true) {
        if(this.output.length==1)
        {
          this.output =0
        }
        else{
          if(this.isOper(this.output[this.output.length-1]))
          {
            this.opflag = false
            this.output = this.output.substr(0,this.output.length-1);
          }
          else{
          this.output = this.output.substr(0,this.output.length-1);
          }
        }
        
      }
    },
    calculate() {
      let result = this.output.replace(new RegExp('×', 'g'), '*').replace(new RegExp('÷', 'g'), '/')
      
      this.output = parseFloat(eval(result).toFixed(9)).toString()
      this.inputflag = false
      this.opflag = false
    },
    calToggle(){
      if (this.opflag || !this.outflag) {
        return
      }
      
      this.output = this.output + '* -1'
      this.calculate()
    },
    clickKey (event) {
      console.log(event)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.calculator {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(6, 1fr);
  height: 18rem;
  width: 25rem;
  margin: auto;
}
.calculator > * {
  border: 1px solid black;
  height: 3rem;
  line-height: 3rem;
  color: white;
}
button{
  cursor: pointer;
  user-select: none;
}
.result {
  grid-column: 1 / span 4;
  grid-row: 1 / span 1;
  background-color: black;
  color: white;
}
.btn_num_0 {
  grid-column: 1 / span 2;
  grid-row: 6 / span 1;
}
.btn_ac,
.btn_pm,
.btn_del {
  background-color: #F2F2F2;
  color: black;
}
.btn_num_1,
.btn_num_2,
.btn_num_3,
.btn_num_4,
.btn_num_5,
.btn_num_6,
.btn_num_7,
.btn_num_8,
.btn_num_9,
.btn_num_0,
.btn_dot {
  background-color: gray;
  font-size: 20px;  
}
.btn_div,
.btn_mul,
.btn_sub,
.btn_pls,
.btn_eq {
  background-color: orange;
  font-size: 20px; 
}

</style>
