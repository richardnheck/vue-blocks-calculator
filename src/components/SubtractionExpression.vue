<template>
  <div class="expression">
     <number-input
      :value="number1"
      @change="updateNumber1($event)"
    />
    <div class="operator">-</div>
   <number-input
      :value="number2"
      @change="updateNumber2($event)"
    />
    <div class="operator">=</div>
    <number-input
      :value="answer"
      @change="updateAnswer($event)"
    />
    <div v-if="expressionComplete" style="width:40px">
      <div class="tick" v-if="answerCorrect">✓</div>
      <div class="cross" v-else>✗</div>
    </div>
    <button class="clear-button" @click="clearExpression()">Clear</button>
  </div>
</template>

<script>
import NumberInput from "./NumberInput.vue";
export default {
  name: "SubtractionExpression",
  components: { NumberInput },
  data: function () {
    return {
      number1: "",
      number2: "",
      answer: "",
    };
  },
  computed: {
    expressionComplete() {
      return (
        this.number1 !== "" &&
        this.number2 !== "" &&
        this.answer !== ""
      );
    },

    answerCorrect() {
      if (this.expressionComplete) {
        return (
          parseInt(this.number1) - parseInt(this.number2) ==
          parseInt(this.answer)
        );
      }
      return false;
    },
  },
  methods: {
    updateNumber1(value) {
      this.number1 = value;
      this.update();
    },

    updateNumber2(value) {
      this.number2 = value;
      this.update();
    },

    updateAnswer(value) {
      this.answer = value;
      this.update();
    },

    clearExpression() {
      this.number1 = "";
      this.number2 = "";
      this.answer = "";
      this.update();
    },

    update() {
      this.$emit("changed", {
        number1: this.number1,
        number2: this.number2,
      });
    },
  },
};
</script>


<style scoped>
</style>
