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
        /
      </button>
    </div>
    <main class="section coral">
      <div class="number-section">
        <!-- NUMBER ONLY -->
        <div v-if="displayType == DisplayType.NUMBER" style="display: flex">
          <number-input @change="show($event)"/>
        </div>

        <!-- MULTIPLICATION -->
        <div v-if="displayType == DisplayType.MULTIPLICATION">
          Multiplication (TODO)
        </div>

        <!-- DIVISION -->
        <div v-if="displayType == DisplayType.DIVISION">Division (TODO)</div>

        <!-- ADDITION -->
        <div v-if="displayType == DisplayType.ADDITION">Addition (TODO)</div>

        <!-- SUBTRACTION -->
        <div v-if="displayType == DisplayType.SUBTRACTION">
          Subtraction (TODO)
        </div>
      </div>
      <div class="blocks-section">
        <div class="place-value-container">
          <div class="block">
            <h2>Thousands</h2>
            <div class="value">{{ thousands }}</div>
          </div>
          <div class="block">
            <h2>Hundreds</h2>
            <div class="value">{{ hundreds }}</div>
          </div>
          <div class="block">
            <h2>Tens</h2>
            <div class="value">{{ tens }}</div>
          </div>
          <div class="block">
            <h2>Ones</h2>
            <div class="value">{{ ones }}</div>
          </div>
        </div>
      </div>
    </main>
    <!--<div class="right-side yellow section" contenteditable>Right Sidebar</div>-->
    <footer class="green section">Parabol Ink ©</footer>
  </div>
</template>

<script>
import NumberInput from './components/NumberInput.vue';

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
  },
  data: function () {
    return {
      displayType: DisplayType.NUMBER,
      number: "",
      thousands: 0,
      hundreds: 0,
      tens: 0,
      ones: 0,
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
  computed: {},

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
      this.ones = 0;
      this.tens = 0;
      this.hundreds = 0;
      this.thousands = 0;
      const length = this.number.length;

      if (length > 0) {
        this.ones = this.number[length - 1];
      }

      if (length > 1) {
        this.tens = this.number[length - 2];
      }

      if (length > 2) {
        this.hundreds = this.number[length - 3];
      }

      if (length > 3) {
        this.thousands = this.number[length - 4];
      }
    },
  },
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
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
  background-color: gray;
  color: white;
}

.left-side {
  grid-column: 1 / 2;
  width: 100px;
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
  width: 95px;
  height: 95px;
}

.left-side button.selected {
  background-color: lightgreen;
}

main {
  grid-column: 2 / 3;
  display: grid;
  grid-template: auto 1fr / auto;
}

main .number-section {
  height: 100px;
  background-color: azure;
}

main .blocks-section {
  background-color: lightgrey;
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
</style>
