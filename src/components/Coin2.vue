<template>
  <div id="coinContainer">
    <div id="coin" ref="coin" @click="startFlip" @animationend="nextAnimation($event)">
      <div class="coinFace sideA"><span>H</span></div>
      <div class="coinFace sideB"><span>T</span></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      result: "",
      coinState: "still",
    };
  },

  computed: {
    coin() {
      return this.$refs.coin;
    },
  },

  methods: {
    startFlip() {
      this.coin.classList = []; //Removes conflicting animation classes if present after flip.
      this.coin.classList.add("flip");
      this.getFlipResult();
    },
    nextAnimation(event) {
      let coin = this.coin;
      let finishedAnimation = event.animationName;

      if (finishedAnimation === "flip") {
        coin.classList.remove(finishedAnimation);
        return coin.classList.add("float");
      } else if (finishedAnimation === "float") {
        coin.classList.remove(finishedAnimation);

        if (this.result === "heads") {
          return coin.classList.add("resultHeads");
        } else {
          return coin.classList.add("resultTails")
        }
      } else {
        return;
      }
    },

    getFlipResult() {
      let result = Math.round(Math.random());

      if (result === 0) {
        return (this.result = "heads");
      } else {
        return (this.result = "tails");
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
  display: flex;
  justify-content: center;
  align-items: center;
}
.sideA {
  background-color: #8F94A2;
  z-index: 100;
}
.sideB {
  background-color: #8F94A2;
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

.endFlip {
  animation: flip 1s linear reverse;
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