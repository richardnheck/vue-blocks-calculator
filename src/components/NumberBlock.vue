<template>
  <div class="container">
    <div class="place-group thousands" v-if="number.length == 4">
      <h2>thousands <span>({{ thousands }})</span></h2>
      <div class="blocks">
        <div v-for="block in this.thousands" :key="block" class="thousand-block"></div>
      </div>
    </div>
    <div class="place-group hundreds" v-if="number.length >= 3">
      <h2>hundreds <span>({{ hundreds }})</span></h2>
      <div class="blocks">
        <div v-for="block in this.hundreds" :key="block" class="hundred-block"></div>
      </div>
    </div>
    <div class="place-group tens" v-if="number.length > 1">
      <h2>tens <span>({{ tens }})</span></h2>
      <div class="blocks">
        <div v-for="block in this.tens" :key="block" class="ten-block"></div>
      </div>
    </div>
    <div class="place-group ones" v-if="number.length > 0">
      <h2>ones <span>({{ ones }})</span></h2>
      <div class="blocks">
        <div v-for="block in this.ones" :key="block" class="one-block"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "NumberBlock",
  props: {
    number:String
  },
  created() {
      this.parse(this.number);
      console.log('NumberBlock', this.number)
  },
  watch: {
    number: function (newVal) {
      this.parse(newVal);
    },
  },
  data: function () {
    return {
      ones: 0,
      tens: 0,
      hundreds: 0,
      thousands: 0,
    };
  },
  methods: {
    parse(number) {
      let num = parseInt(number).toString()
      this.ones = 0;
      this.tens = 0;
      this.hundreds = 0;
      this.thousands = 0;
      const length = num.length;
      console.log('length', length)

      if (length > 0) {
        this.ones = parseInt(num[length - 1]);
      }

      if (length > 1) {
        this.tens = parseInt(num[length - 2]);
      }

      if (length > 2) {
        this.hundreds = parseInt(num[length - 3]);
      }

      if (length > 3) {
        this.thousands = parseInt(num[length - 4]);
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.container {
  display: flex;
  flex-direction: row;
  

  .place-group {
    border: 1px solid lightgray;
    flex-grow: 1;
    margin:5px;
    //align-items: stretch;
    max-width:350px;

    h2 {
      display: block;
      text-align: center;
      font-size:0.7rem;
      margin:0px;
      font-weight: 100;

      span {
        color: gray;
        font-size: 0.7rem;
      }
    }

    .blocks {
      display: flex;
      flex-flow: row wrap;
      justify-content: flex-start;
      min-width: 100px;
      max-width: 500px;
      padding:5px;

    
      .one-block {
        display: block;
        margin: 2px;
        width: 20px;
        height: 20px;
        background-image: url("../assets/one-block.png");
        background-repeat: no-repeat;
        background-size: cover;
      }

      .ten-block {
        display: block;
        margin: 2px;
        width:19px;
        height:135px;
        background-image: url("../assets/ten-block.png");
        background-repeat: no-repeat;
        background-size: cover;
      }

      .hundred-block {
        display: block;
        margin: 2px;
        width:135px;
        height:40px;
        background-image: url("../assets/hundred-block.png");
        background-repeat: no-repeat;
        background-size: cover;
      }

      .thousand-block {
        display: block;
        margin: 2px;
        width:100px;
        height:90px;
        background-image: url("../assets/thousand-block.png");
        background-repeat: no-repeat;
        background-size: cover;
      }
    }
  }
}
</style>
