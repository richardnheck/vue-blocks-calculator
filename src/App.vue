<template>
  <div id="app">
    <keyboard
      style="float: right"
      @clear="onClear()"
      @delete="onDelete()"
      @number="onNumber($event)"
    />
    <HelloWorld msg="Welcome to Your Blocks Calculator" />
    <div style="display: flex">
      <input
        type="number"
        readonly
        maxlength="4"
        class="number-input"
        v-model="number"
      />
      <button class="show-button" @click="show()">Show</button>
    </div>
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
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import Keyboard from "./components/Keyboard.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
    Keyboard,
  },
  data: function () {
    return {
      number: "",
      thousands: 0,
      hundreds: 0,
      tens: 0,
      ones: 0,
    };
  },
  computed: {},

  methods: {
    /**
     * Show the number blocks
     */
    show() {
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

    /**
     * Handle when a number is pressed on the virtual keyboard
     */
    onNumber(value) {
      if (this.number.length < 4) {
        this.number += value;
      }
    },

    /**
     * Clear the number
     */
    onClear() {
      this.number = "";
    },

    /**
     * Delete the last character of the number
     */
    onDelete() {
      this.number = this.number.slice(0, -1);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

.place-value-container {
  display: flex; /* or inline-flex */
}

.place-value-container .block {
  width: 200px;
}

.show-button {
  width: 150px;
  height: 150px;
}
.number-input {
  font-size: 120px;
  width: 400px;
}
</style>
