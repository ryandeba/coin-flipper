<script>
  export default {
    name: 'App',
    components: {
      guessbtn: require("./components/GuessBtn").default,
      cointwo: require("./components/Coin2").default
    },

    data() {
      return {
        guess: "",
        score: 0,
        result: ""
      }
    },

    methods: {
      onEndFlip() {
        if (this.guess === this.result) {
          this.score++;
        } else {
          this.score--;
        }

        this.guess = "";
        this.result = "";
      },
      setResult() {
        
      let result = Math.round(Math.random());

      if (result === 0) {
        return this.result = "heads";
      } else {
        return this.result = "tails";
      
    }
      }
    }
  };
</script>

<template>
  <div>
    <cointwo @flip-started="setResult" @flip-ended="onEndFlip" :guess="guess" :result="result"></cointwo>
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

<style>
  body {
    margin: 0;
    padding: 0;
  }
</style>
