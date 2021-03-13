<template>
  <div>
    <div
      class="panel panel--heads"
      @mouseover="input = 'heads'"
    >
      Heads
    </div>

    <div 
      class="panel panel--tails"
      @mouseover="input = 'tails'"
    >
      Tails
    </div>
  </div>

</template>

<script>
  export default {
    props: ["value"],

    data() {
      return {
        input: ""
      };
    },

    watch: {
      input() {
        this.$emit("input", this.input);
      }
    },
  }
</script>

<style scoped>
  .panel {
    position: fixed;
    top: 0;
    width: 50vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: flex-end;
    font-size: 50px;
    color: #fff;
    opacity: 0.4;
    transition: all 0.2s;
  }

  .panel:hover {
    opacity: 0.8;
  }

  .panel--heads {
    background-color: #bb0000;
    animation: slide-right 0.3s ease forwards;
  }

  .panel--tails {
    background-color: #3e3e3e;
    animation: slide-left 0.3s ease forwards;
  }

  @keyframes slide-right {
    0% { left: -100%; }
    100% { left: 0%; }
  }

  @keyframes slide-left {
    0% { right: -100%; }
    100% { right: 0; }
  }
</style>
