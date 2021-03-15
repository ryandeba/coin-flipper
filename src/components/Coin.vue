<template>
  <div id="coinContainer">
    <div
      id="coin"
      ref="coin"
      @click="startFlip"
      @animationstart="currentAnimation = $event.animationName"
      @animationend="nextAnimation($event)"
    >
      <div class="coinFace sideA"><span>H</span></div>
      <div class="coinFace sideB"><span>T</span></div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["guess", "result"],

  data() {
    return {
      currentAnimation: ""
    }
  },

  computed: {
    coin() {
      return this.$refs.coin;
    },
    finishAnimation() {
      if (this.guess !== "" &&
          this.currentAnimation.includes("float")) {
        return true;
      } else{
        return false;
      }
    }
  },

  methods: {
    startFlip() {
      this.coin.classList = []; //Removes conflicting animation classes if present after flip.
      this.coin.classList.add("flip");

      this.$emit("flip-started");
    },

    nextAnimation(event) {
      let coin = this.coin;
      let finishedAnimation = event.animationName;

      if (finishedAnimation.includes("flip")) {
        coin.classList.remove(finishedAnimation);
        return coin.classList.add("float");
      } else if (finishedAnimation.includes("float")) {
        coin.classList.remove(finishedAnimation);

        if (this.result === "heads") {
          return coin.classList.add("heads");
        } else {
          return coin.classList.add("tails");
        }
      } else {
        return this.$emit('flip-ended');
      }
    }
  },

  watch: {
    finishAnimation: function() {
      if (this.finishAnimation) {
        return this.nextAnimation({animationName: "float"});
      }
    }
  }
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
  height: 100px;
  position: absolute;
  transition: transform 1s;
  transform-style: preserve-3d;
  z-index: 1000;
}
.coinFace {
  width: 100%;
  height: 100%;
  background-color: #8f94a2;
  border-radius: 50%;
  position: absolute;
  backface-visibility: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}
.sideA {
  z-index: 100;
}
.sideB {
  transform: rotateX(-180deg);
}
span {
  font-size: 3rem;
}
.flip {
  animation: flip 1s linear;
}
.float {
  animation-name: float;
  animation-duration: 10s;
  animation-timing-function: linear;
}
.heads {
  animation: resultHeads 1s linear forwards;
}
.tails {
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
    transform: rotateX(1980deg);
    bottom: 25vh;
  }
}
@keyframes resultHeads {
  from {
    transform: rotateX(0);
    bottom: calc(100% - 100px);
  }
  to {
    transform: rotateX(1800deg);
    bottom: 25vh;
  }
}
</style>