<template>
  <div id="app" class="parent">
    <header class="pink section">Blocks Calculator</header>
    <div class="left-side section">
      <button
        :class="{ selected: displayType == DisplayType.NUMBER }"
        @click="selectDisplayType(DisplayType.NUMBER)"
        title="Number"
      >
        123
      </button>
      <button
        :class="{ selected: displayType == DisplayType.ADDITION }"
        @click="selectDisplayType(DisplayType.ADDITION)"
        title="Addition"
      >
        +
      </button>
      <button
        :class="{ selected: displayType == DisplayType.SUBTRACTION }"
        @click="selectDisplayType(DisplayType.SUBTRACTION)"
        title="Subtraction"
      >
        -
      </button>
      <button
        :class="{ selected: displayType == DisplayType.MULTIPLICATION }"
        @click="selectDisplayType(DisplayType.MULTIPLICATION)"
        title="Multiplication"
      >
        x
      </button>
      <button
        :class="{ selected: displayType == DisplayType.DIVISION }"
        @click="selectDisplayType(DisplayType.DIVISION)"
        title="Division"
      >
        ÷
      </button>
    </div>
    <main class="section coral">
      <div class="number-section">
        <!-- NUMBER ONLY -->
        <div
          v-show="displayType == DisplayType.NUMBER"
          style="display: flex; padding: 5px"
        >
          <span style="font-size: 40px; margin-top: 5px; margin-right: 10px"
            >Enter Number</span
          >
          <number-input :value="number" @change="show($event)" />
        </div>

        <!-- MULTIPLICATION -->
        <div v-show="displayType == DisplayType.MULTIPLICATION">
          <multiplication-expression @changed="updateMultiplication($event)" />
        </div>

        <!-- DIVISION -->
        <div v-show="displayType == DisplayType.DIVISION">
          <division-expression @changed="updateDivision($event)" />
        </div>

        <!-- ADDITION -->
        <div v-show="displayType == DisplayType.ADDITION">
          <addition-expression @changed="updateAddition($event)" />
        </div>

        <!-- SUBTRACTION -->
        <div v-show="displayType == DisplayType.SUBTRACTION">
          <subtraction-expression @changed="updateSubtraction($event)"/>
        </div>
      </div>
      <div class="blocks-section">
        <!-- NUMBER ONLY -->
        <div v-if="displayType == DisplayType.NUMBER">
          <number-block :number="this.number" />
        </div>

        <!-- MULTIPLICATION -->
        <div
          v-if="displayType == DisplayType.MULTIPLICATION"
          style="padding: 10px"
        >
          <div
            v-for="n in multiplier"
            :key="n"
            style="
              border: 1px solid black;
              min-height: 100px;
              margin-bottom: 5px;
            "
          >
            <number-block :number="multiplicationNumber" />
          </div>
        </div>

        <!-- DIVISION -->
        <div v-if="displayType == DisplayType.DIVISION" style="padding: 10px">
          <number-block :number="divisionNumber" />
          <hr v-if="divisionNumber != ''" />
          <div
            class="error"
            v-if="divisionRemainder !== '' && parseInt(divisionRemainder) < 0"
          >
            Your answer is too big
          </div>
          <div v-if="divisionAnswer !== '' && divisionRemainder > 0" style="">
            <span>R</span><number-block :number="divisionRemainder" />
          </div>
          <div
            v-if="
              divisor !== '' &&
              divisionNumber !== '' &&
              parseInt(divisor) <= parseInt(divisionNumber)
            "
            style="margin-top: 20px"
          >
            <div
              v-for="n in divisor"
              :key="n"
              style="
                border: 1px solid black;
                min-height: 100px;
                margin-bottom: 5px;
              "
            >
              <number-block
                v-if="divisionAnswer !== ''"
                :number="divisionAnswer"
              />
            </div>
          </div>
        </div>

        <!-- ADDITION -->
        <div v-if="displayType == DisplayType.ADDITION">
          <number-block :number="additionNumber1" /> 
          <div style="font-size:4rem" v-if="additionNumber1">+</div>
          <number-block :number="additionNumber2" />
        </div>

        <!-- SUBTRACTION -->
        <div v-if="displayType == DisplayType.SUBTRACTION">
          <number-block :number="subtractionNumber1" /> 
          <div style="font-size:4rem" v-if="subtractionNumber1">-</div>
          <number-block :number="subtractionNumber2" />
        </div>
      </div>
    </main>
    <!--<div class="right-side yellow section" contenteditable>Right Sidebar</div>-->
    <footer class="green section">Parabol Ink ©</footer>
  </div>
</template>

<script>
import AdditionExpression from "./components/AdditionExpression.vue";
import DivisionExpression from "./components/DivisionExpression.vue";
import MultiplicationExpression from "./components/MultiplicationExpression.vue";
import NumberBlock from "./components/NumberBlock.vue";
import NumberInput from "./components/NumberInput.vue";
import SubtractionExpression from './components/SubtractionExpression.vue';

const DisplayType = {
  NUMBER: "number",
  DIVISION: "division",
  MULTIPLICATION: "multiplication",
  ADDITION: "addition",
  SUBTRACTION: "subtraction",
};

export default {
  name: "App",
  components: {
    NumberInput,
    NumberBlock,
    MultiplicationExpression,
    DivisionExpression,
    AdditionExpression,
    SubtractionExpression
  },
  data: function () {
    return {
      displayType: DisplayType.NUMBER,

      // For Number Only Display
      number: "",

      // For Multiplication
      multiplicationNumber: "",
      multiplier: "",

      // For Division
      divisionNumber: "",
      divisor: "",
      divisionAnswer: "",

      // For Addition
      additionNumber1: "",
      additionNumber2: "",

      // For Subtraction
      subtractionNumber1: "",
      subtractionNumber2: "",
    };
  },

  /**
   * Handle when component is created
   */
  created() {
    this.DisplayType = DisplayType;
  },

  /**
   * Computed Properties
   */
  computed: {
    /**
     * Get the remainder given the answer provided
     */
    divisionRemainder() {
      if (
        this.divisionNumber !== "" &&
        this.divisor !== "" &&
        this.divisionAnswer
      ) {
        return (
          parseInt(this.divisionNumber) -
          parseInt(this.divisor) * parseInt(this.divisionAnswer)
        ).toString();
      }
      return "";
    },

    correctDivisionAnswer() {
      if (this.divisionNumber !== "" && this.divisor !== "") {
        return Math.floor(
          parseInt(this.divisionNumber) / parseInt(this.divisor)
        ).toString();
      }
      return "";
    },

    correctDivisionRemainder() {
      if (this.divisionNumber !== "" && this.divisor !== "") {
        return (
          parseInt(this.divisionNumber) % parseInt(this.divisor)
        ).toString();
      }
      return "";
    },
  },

  /**
   * Methods
   */
  methods: {
    /**
     * Select the type of Display (e.g number, addition, multiplication etc)
     */
    selectDisplayType(displayType) {
      this.displayType = displayType;
    },

    /**
     * Show the number blocks
     */
    show(number) {
      this.number = number;
    },

    /**
     * Update the multiplication blocks display
     */
    updateMultiplication({ multiplier, number }) {
      this.multiplier = multiplier;
      this.multiplicationNumber = number;
    },

    /**
     * Update the division blocks display
     */
    updateDivision({ divisor, number, answer }) {
      this.divisor = divisor;
      this.divisionNumber = number;
      this.divisionAnswer = answer;
    },

    /**
     * Update the addition blocks display
     */
    updateAddition({ number1, number2 }) {
      console.log('update addition', number1, number2)
      this.additionNumber1 = number1;
      this.additionNumber2 = number2;
    },

    /**
     * Update the subtraction blocks display
     */
    updateSubtraction({ number1, number2 }) {
      console.log('update subtraction', number1, number2)
      this.subtractionNumber1 = number1;
      this.subtractionNumber2 = number2;
    },
  },
};
</script>

<style lang="scss">
html,
body {
  margin: 0 !important;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.expression {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 10px;
  padding: 5px;
  height: 65px;

  .operator {
    font-size: 4rem;
  }

  select {
    font-size: 3rem;
    height: 65px;
  }

  button {
    height: 100%;
  }

  .tick,
  .cross {
    font-size: 3rem;
  }
  .tick {
    color: green;
  }
  .cross {
    color: red;
  }

  .clear-button {
    margin-left: auto;
  }
}

.parent {
  display: grid;
  grid-template: auto 1fr auto / auto 1fr auto;
  width: 100vw;
  height: 100vh;
}

header {
  padding: 0.5rem;
  grid-column: 1 / 4;
  background-color: black;
  color: white;
}

.left-side {
  grid-column: 1 / 2;
  width: 70px;
  background-color: lightblue;
  display: flex;
  flex-direction: row;
  flex-flow: wrap;
  justify-content: flex-start;
  flex-wrap: wrap;
  flex-direction: row;
  align-content: flex-start; /* NEW */
  padding: 5px;
}

.left-side button {
  margin-bottom: 5px;
  width: 70px;
  height: 70px;
  font-size: 2rem;
}

.left-side button.selected {
  background-color: gold;
}

main {
  grid-column: 2 / 3;
  display: grid;
  grid-template: auto 1fr / auto;
}

main .number-section {
  height: 80px;
  background-color: azure;
}

main .blocks-section {
  background-color: white;
}

.right-side {
  grid-column: 3 / 4;
  background-color: lightblue;
}

footer {
  grid-column: 1 / 4;
  background-color: black;
  text-align: center;
  color: white;
}

.place-value-container {
  display: flex; /* or inline-flex */
}

.place-value-container .block {
  min-width: 150px;
}

.show-button {
  width: 100px;
  height: 100%;
}

.error {
  color: red;
}
</style>
