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
    </div>
    <div class="control-buttons" style="width:100px">
    <button @click="deleteClicked()">X</button>
    <button @click="clearClicked()">Clear</button>
    <button @click="doneClicked()">Done</button>
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
<style scoped>
.keyboard {
  width: 500px;
  border: 1px solid gray;
  display: flex; /* or inline-flex */
}

.overlay {
  position: fixed;
  z-index: 1000;
  text-align: left;
  box-shadow: 0px 0px 35px -16px rgba(0, 0, 0, 0.75);
  padding: 5px;
  color: #363b41;
  background-color: #ffffff;
  overflow: visible;
}

.number-buttons {
  width: 350px;
  padding: 5px;
}
button {
  width: 100px;
  height: 100px;
  margin: 1px;
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
