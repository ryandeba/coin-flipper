<script>
  export default {
    data() {
      return {
        classes: ""
      };
    },

    methods: {
      flip() {
        this.$emit('startFlip');

        let result = Math.random() <= 0.5 ? "heads" : "tails";

        setTimeout(() => {
          this.$emit('endFlip', result)
        }, 3000)

        this.classes = "";
        setTimeout(() => {
          this.classes = result;
        }, 0);
      }
    }
  };
</script>

<template>
  <div style="position: relative; height: 75vh; display: flex; justify-content: center;">
    <div
      id="coin"
      :class="classes"
      @click="flip"
      style="position: absolute; bottom: 0; display: flex; font-size: 20px;"
    >
      <div class="side-a" style="text-align: center;">heads</div>
      <div class="side-b" style="text-align: center;">tails</div>
    </div>
  </div>
</template>

<style lang="css" scoped>
#coin {
  position: relative;
  margin: 0 auto;
  width: 100px;
  height: 100px;
  cursor: pointer;
}
#coin div {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  box-shadow: inset 0 0 45px rgba(255,255,255,.3), 0 12px 20px -10px rgba(0,0,0,.4);
}
.side-a {
  background-color: #bb0000;
}
.side-b {
  background-color: #3e3e3e;
}

#coin {
  transition: -webkit-transform 1s;
  -webkit-transform-style: preserve-3d;
}
#coin div {
  position: absolute;
  -webkit-backface-visibility: hidden;
}
.side-a {
  z-index: 100;
}
.side-b {
  -webkit-transform: rotateX(-180deg);

}

#coin.heads {
  animation: flipHeads 3s linear forwards;
}
#coin.tails {
  animation: flipTails 3s linear forwards;
}

@keyframes flipHeads {
  0% {
    transform: rotateX(0);
    bottom: 0;
  }
  50% {
    bottom: calc(100% - 100px);
  }
  100% {
    transform: rotateX(1800deg);
    bottom: 0;
  }
}
@keyframes flipTails {
  0% {
    transform: rotateX(0);
    bottom: 0;
  }
  50% {
    bottom: calc(100% - 100px);
  }
  100% {
    transform: rotateX(1980deg);
    bottom: 0;
  }
}
</style>
