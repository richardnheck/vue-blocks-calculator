<template>
  <div class="expression">
    <select v-model="multiplier">
      <option v-for="option in options" :value="option" :key="option">
        {{ option }}
      </option>
    </select>
    <div class="operator">X</div>
    <number-input
      :value="number"
      @change="updateNumber($event)"
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
  name: "MultiplicationExpression",
  props: {
    msg: String,
  },
  watch: {
    multiplier: function () {
      this.update();
    },
  },
  components: { NumberInput },
  data: function () {
    return {
      number: "",
      multiplier: "",
      options: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
      answer: "",
    };
  },
  computed: {
    expressionComplete() {
      return (
        this.number != "" &&
        this.multiplier != "" &&
        this.answer != ""
      );
    },

    answerCorrect() {
      if (this.expressionComplete) {
        return (
          parseInt(this.multiplier) * parseInt(this.number) ==
          parseInt(this.answer)
        );
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

    clearExpression() {
      this.multiplier = "";
      this.number = "";
      this.answer = "";
      this.update();
    },

    update() {
      this.$emit("changed", {
        multiplier: this.multiplier,
        number: this.number,
      });
    },
  },
};
</script>


<style scoped>
</style>
