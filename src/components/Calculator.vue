<template>
  <div>
    <div id="calculator" class="calculator">
      <div class="result">{{ displayed == '' ? 0 : displayed }}</div>
      <div @click="opposite" class="btn">+/-</div>
      <div @click="clear" class="btn">AC</div>
      <div @click="percent" class="btn">%</div>
      <div @click="calculate('/')" class="btn">÷</div>
      <div @click="append(7)" class="btn">7</div>
      <div @click="append(8)" class="btn">8</div>
      <div @click="append(9)" class="btn">9</div>
      <div @click="calculate('*')" class="btn">X</div>
      <div @click="append(4)" class="btn">4</div>
      <div @click="append(5)" class="btn">5</div>
      <div @click="append(6)" class="btn">6</div>
      <div @click="calculate('-')" class="btn">-</div>
      <div @click="append(1)" class="btn">1</div>
      <div @click="append(2)" class="btn">2</div>
      <div @click="append(3)" class="btn">3</div>
      <div @click="calculate('+')" class="btn">+</div>
      <div @click="append(0)" class="btn zero">0</div>
      <div @click="dot" class="btn">.</div>
      <div @click="axiosCall" v-on:click="equal" class="btn">=</div>
    </div>
    <div class="quote">{{quote}}</div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  el: "#calculator",
  data() {
    return {
      displayed: "",
      previous: "",
      result: "",
      quote: "",
      isOperating: false
    };
  },
  methods: {
    append(num) {
      if (this.isOperating) {
        this.displayed = "";
        this.isOperating = false;
      }
      this.displayed += num;
    },
    clear() {
      this.displayed = "";
      this.previous = "";
      this.result = "";
    },
    opposite() {
      this.displayed *= -1;
    },
    dot() {
      if (this.displayed === "") this.displayed = 0;
      if (this.displayed.indexOf(".") == -1) this.append(".");
    },
    percent() {
      this.displayed /= 100;
    },
    calculate(operator) {
        if(this.previous == ""){
          this.isOperating = true;
          this.previous = this.displayed
        }
        if(this.isOperating && this.previous != this.displayed)
        this.result = eval(`${this.previous} ${operator} ${this.displayed}`)
        console.log(this.previous, this.displayed)
    },
    equal() {
      this.displayed = this.result;
      this.previous = "";
    },
    axiosCall() {
      axios
        .get("https://api.kanye.rest/")
        .then(res => (this.quote = res.data.quote))
        .catch(err => console.log(err));
    }
  },
};
</script>
<style scoped>
.calculator {
  font-size: 5vh;
  text-align: center;
  width: 50%;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(4, 8vh);
  grid-template-rows: repeat(6, 8vh);
  grid-column-gap: 1vh;
  grid-row-gap: 1vh;
}

.result {
  grid-column: 1/5;
  background-color: black;
  color: white;
  border-radius: 5px;
}

.btn {
  cursor: pointer;
  border: 1px solid black;
  border-radius: 5px;
}
.zero {
  grid-column: 1/3;
  text-align: left;
  padding-left: 1vh;
}
.quote {
  text-align: center;
  margin-top: 5vh;
}
</style>


