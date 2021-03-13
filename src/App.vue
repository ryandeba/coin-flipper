<script>
  export default {
    name: 'App',
    components: {
      coin: require("./components/Coin").default,
      guess: require("./components/Guess").default,
    },

    data() {
      return {
        lastResult: "",
        guess: "",
        score: 0,
        flipping: false
      }
    },

    methods: {
      onStartFlip() {
        this.guess = '';
        this.flipping = true;
      },

      onEndFlip(result) {
        this.lastResult = result;

        if (this.guess === result) {
          this.score++;
        } else {
          this.score--;
        }

        this.flipping = false;
      }
    }
  };
</script>

<template>
  <div class="container">
    <coin @startFlip="onStartFlip" @endFlip="onEndFlip"></coin>

    <div>
      <div v-if="flipping">
        <guess v-model="guess"></guess>
      </div>

      <template v-else-if="Boolean(lastResult)">
        {{ lastResult == guess ? 'Winner!' : 'Loser!' }}
      </template>

      <template v-else>
        Flip the Coin!
      </template>
    </div>

    <div style="position: fixed; text-align: left;">
      <div>My guess: {{ guess }} </div>
      <div>My Score: {{ score }} </div>
    </div>
  </div>
</template>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Open+Sans&display=swap');

  .container {
    display: grid;
    grid-gap: 10px;
    grid-template-rows: 4fr 1fr;
    text-align: center;

    height: 100vh;
    width: 100vw;
    position: fixed;
    top: 0;
    left: 0;

    background-color: #eee;
  }

  * {
    font-size: 20px;
    font-family: 'Open Sans', sans-serif;
    color: #444;
  }
</style>
