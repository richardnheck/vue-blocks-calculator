<template>
  <div class="keyboard overlay">
    <div class="number-buttons">
      <button
        class="number-button"
        v-for="number in numbers"
        :key="number"
        @click="numberClicked(number)"
      >
        {{ number }}
      </button>
      <button class="number-button control delete" @click="deleteClicked()">&lt;</button>
      <button class="control clear" @click="clearClicked()">C</button>
    </div>
    <div class="control-buttons">
      
      <button class="done" @click="doneClicked()">OK</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Keyboard",
  data: function () {
    return {
      numbers: [7, 8, 9, 4, 5, 6, 1, 2, 3, 0],
    };
  },
  methods: {
    numberClicked(number) {
      this.$emit("number", number);
    },

    clearClicked() {
      this.$emit("clear");
    },

    deleteClicked() {
      this.$emit("delete");
    },

    doneClicked() {
      this.$emit("done");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.keyboard {
  width: 340px;
  border: 2px solid #322348;
  display: flex; /* or inline-flex */
  border-radius: 5px;
}

.overlay {
  position: fixed;
  z-index: 1000;
  text-align: left;
  box-shadow: 0px 0px 35px -16px rgba(0, 0, 0, 0.9);
  padding: 5px;
  color: #363b41;
  background-color: #563d7c;
  overflow: visible;
}

.number-buttons {
  width: 250px;
  padding: 5px;
}

.control-buttons {
  width:75px;
  padding: 5px;
}

button {
  width: 70px;
  height: 70px !important;
  margin: 5px;
  background-color:transparent;
  border: 1px solid #ffe484;
  color:#ffe484;
  font-size:2rem;
  border-radius:5px;

  &:active {
    background-color: #7755aa;
  }
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
