<template>
  <div class="expression">
    <number-input :value="number" @change="updateNumber($event)" />
    <div class="operator">÷</div>
    <select v-model="divisor">
      <option v-for="option in options" :value="option" :key="option">
        {{ option }}
      </option>
    </select>
    <div class="operator">=</div>
    <number-input :value="answer" @change="updateAnswer($event)" />
    <div class="operator">R</div>
    <number-input :value="answerRemainder" @change="updateAnswerRemainder($event)" />
    <div v-if="expressionComplete" style="width: 40px">
      <div class="tick" v-if="answerCorrect">✓</div>
      <div class="cross" v-else>✗</div>
    </div>
    <button class="clear-button" @click="clearExpression()">Clear</button>
  </div>
</template>

<script>
import NumberInput from "./NumberInput.vue";
export default {
  name: "DivisionExpression",
  props: {
    msg: String,
  },
  watch: {
    divisor: function () {
      this.update();
    },
  },
  components: { NumberInput },
  data: function () {
    return {
      number: "",
      divisor: "",
      options: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
      answer: "",
      answerRemainder: ""
    };
  },
  computed: {
    expressionComplete() {
      return this.number != "" && this.divisor != "" && this.answer != "" && this.answerRemainder != '';
    },

    answerCorrect() {
      if (this.expressionComplete) {
          let actualAnswer = parseInt(this.number) / parseInt(this.divisor)
          let actualRemainder =  parseInt(this.number) % parseInt(this.divisor)
          console.log('Actual answer', actualAnswer, actualRemainder)
          if(actualRemainder == 0) {
            return Math.floor(actualAnswer) == this.answer;
          } else {
            return Math.floor(actualAnswer) == this.answer && actualRemainder == this.answerRemainder;
          }
      }
      return false;
    },

    divisionError() {
      if (this.number != "" && this.divisor != "") {
        return parseInt(this.divisor) > parseInt(this.number);
      }
      return false;
    },
  },
  methods: {
    updateNumber(value) {
      this.number = value;
      this.update();
    },

    updateAnswer(value) {
      this.answer = value;
      this.update();
    },

    updateAnswerRemainder(value) {
      this.answerRemainder = value;
      this.update();
    },

    clearExpression() {
      this.divisor = "";
      this.number = "";
      this.answer = "";
      this.answerRemainder = "";
      this.update();
    },

    update() {
      this.$emit("changed", {
        divisor: this.divisor,
        number: this.number,
        answer: this.answer
      });
    },
  },
};
</script>


<style scoped>
.error {
  color: red;
}
</style>
