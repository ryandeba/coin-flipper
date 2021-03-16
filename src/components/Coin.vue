<template>
  <div id="coinContainer">
    <div
      id="coin"
      ref="coin"
      @click="startFlip"
      @animationstart="currentAnimation = $event.animationName"
      @animationend="nextAnimation"
    >
      <div class="side heads"></div>
      <div class="side tails"></div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["guess", "result"],

  data() {
    return {
      currentAnimation: "",
    };
  },

  computed: {
    coin() {
      return this.$refs.coin;
    },
    finishAnimation() {
      if (this.guess !== "" && this.currentAnimation.includes("float")) {
        return true;
      } else {
        return false;
      }
    },
  },

  methods: {
    startFlip() {
      this.coin.classList = []; //Removes conflicting animation classes if present after flip.
      this.coin.classList.add("flip");

      this.$emit("flip-started");
    },

    nextAnimation() {
      let coin = this.coin;

      if (this.currentAnimation.includes("flip")) {
        coin.classList.remove("flip");
        return coin.classList.add("float");
      } else if (this.currentAnimation.includes("float")) {
        coin.classList.remove("float");

        if (this.result === "heads") {
          return coin.classList.add("resultHeads");
        } else {
          return coin.classList.add("resultTails");
        }
      } else {
        return this.$emit("flip-ended");
      }
    },
  },

  watch: {
    finishAnimation: function () {
      if (this.finishAnimation) {
        return this.nextAnimation();
      }
    },
  },
};
</script>

<style scoped>
#coinContainer {
  height: 75vh;
  display: flex;
  justify-content: center;
  align-items: flex-end;
}
#coin {
  width: 100px;
  height: 10px;
  background: linear-gradient(90deg, silver, #141001);
  position: absolute;
  transform: rotateX(-90deg);
  transform-style: preserve-3d;
  z-index: 1000;
}
.side,
#coin::before,
#coin::after {
  content: "😎";
  font-size: 60px;
  filter: grayscale(50%);
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background: linear-gradient(90deg, silver, #141001);
  position: absolute;
  bottom: calc(-100px / 2 + 10px);
  transform: rotateX(-90deg);
  backface-visibility: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}
.tails,
#coin::after {
  content: "🦅";
  top: calc(-100px / 2 + 10px);
  transform: rotateX(90deg);
}
#coin::before,
#coin::after {
  backface-visibility: hidden;
  transform: rotateX(90deg);
  background: silver;
}
#coin::after {
  transform: rotateX(-90deg);
}

.flip {
  animation: flip 1s linear;
}
.float {
  animation-name: float;
  animation-duration: 10s;
  animation-timing-function: linear;
}
.resultHeads {
  animation: resultHeads 1s linear forwards;
}
.resultTails {
  animation: resultTails 1s linear forwards;
}

@keyframes flip {
  from {
    transform: rotateX(0);
    bottom: 25vh;
  }
  to {
    transform: rotateX(1800deg);
    bottom: calc(100% - 100px);
  }
}
@keyframes float {
  from {
    transform: rotateX(180deg);
    bottom: calc(100% - 100px);
  }
  to {
    transform: rotateX(18000deg);
    bottom: calc(100% - 100px);
  }
}
@keyframes resultTails {
  from {
    transform: rotateX(0);
    bottom: calc(100% - 100px);
  }
  to {
    transform: rotateX(1890deg);
    bottom: 25vh;
  }
}
@keyframes resultHeads {
  from {
    transform: rotateX(0);
    bottom: calc(100% - 100px);
  }
  to {
    transform: rotateX(1710deg);
    bottom: 25vh;
  }
}
</style>