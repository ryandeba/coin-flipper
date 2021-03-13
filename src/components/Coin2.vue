<template>
  <div id="coinContainer">
    <div id="coin" ref="coin" @click="flip" @animationend="testFn">
      <div class="coinFace sideA"></div>
      <div class="coinFace sideB"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      result: "",
      state: "",
    };
  },

  methods: {
    flip() {
      const coin = this.$refs.coin;
      coin.classList.add("flipHeads");
    },
    testFn() {
      const coin = this.$refs.coin;
      if (!this.state) {
        this.state = "flipping";
        coin.classList.remove("flipHeads");
        coin.classList.add("keepFlipping");
      } else {
        coin.classList.remove("keepFlipping");
        coin.classList.add("endFlip");
      }
    },
  },
};
</script>

<style>
#coinContainer {
  height: 75vh;
  display: flex;
  justify-content: center;
  align-items: flex-end;
}
#coin {
  width: 100px;
  height: 100px;
  position: absolute;
  transition: transform 1s;
  transform-style: preserve-3d;
}
.coinFace {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  position: absolute;
  backface-visibility: hidden;
}
.sideA {
  background-color: blue;
  z-index: 100;
}
.sideB {
  background-color: red;
  transform: rotateX(-180deg);
}
.flipHeads {
  animation: flipHeads 1s linear;
}
.keepFlipping {
  animation-name: keepFlipping;
  animation-duration: 10s;
  animation-timing-function: linear;
}

.endFlip {
  animation: flipHeads 1s linear reverse;
}

#coin.tails {
  animation: flipTails 3s infinite;
}
@keyframes flipHeads {
  from {
    transform: rotateX(0);
    bottom: 25vh;
  }
  to {
    transform: rotateX(1800deg);
    bottom: calc(100% - 100px);
  }
}
@keyframes keepFlipping {
  from {
    transform: rotateX(0);
    bottom: calc(100% - 100px);
  }
  to {
    transform: rotateX(18000deg);
    bottom: calc(100% - 100px);
  }
}
@keyframes flipTails {
  from {
    transform: rotateX(0);
    bottom: 0;
  }
  to {
    transform: rotateX(1980deg);
    bottom: 100%;
  }
}
</style>