<template>
  <div>
    <input
      type="number"
      maxlength="4"
      readonly
      class="number-input"
      v-model="number"
      placeholder="____"
      @click="toggleKeyboard()"
    />

    <keyboard
      v-if="keyboardVisible"
      @clear="onClear()"
      @delete="onDelete()"
      @number="onNumber($event)"
      @done="onDone()"
    />
  </div>
</template>

<script>
import Keyboard from "./Keyboard.vue";
export default {
  name: "NumberInput",
  components: { Keyboard },
  data: function () {
    return {
      number: "",
      keyboardVisible: false,
    };
  },
  props: {
    value:String
  },
  created() {
    this.number = this.value;
  },
  methods: {
    onChanged() {
      this.$emit("change", this.number);
    },

    toggleKeyboard() {
      this.keyboardVisible = !this.keyboardVisible;
      if (!this.keyboardVisible) {
        this.onChanged();
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

    /**
     * Done clicked
     */
    onDone() {
      this.keyboardVisible = false;
      this.onChanged();
    },
  },
};
</script>


<style scoped>
.number-input {
  font-size: 50px;
  width: 130px;
}
</style>
