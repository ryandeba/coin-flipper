<script>
  export default {
    name: 'App',
    components: {
      coin: require("./components/Coin").default,
      guessbtn: require("./components/GuessBtn").default,
    },

    data() {
      return {
        guess: "",
        score: 0
      }
    },

    methods: {
      onEndFlip(result) {
        if (this.guess === result) {
          this.score++;
        } else {
          this.score--;
          
        }
      }
    }
  };
</script>

<template>
  <div>
    <coin @startFlip="guess = ''" @endFlip="onEndFlip"></coin>
    <guessbtn @click.native="guess = 'heads'" :disabled='Boolean(guess)'>
      Heads
    </guessbtn>
    <guessbtn @click.native="guess = 'tails'" :disabled='Boolean(guess)'>
      Tails
    </guessbtn>
    <div>My guess: {{ guess }} </div>
    <div>My Score: {{ score }} </div>
  </div>
</template>
