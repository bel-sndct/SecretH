<template>
  <div class="popup_wrapper">
    <div class="v-popup">
      <div class="v-popup__header">
        <p v-for="(text, index) in textOutput" :key="index" id="out">{{text}}</p>
      </div>
      <div class="v-popup__content">

      </div>
      <div class="v-popup__footer">
      <button @click="restartGame()" class="restartButton">Restart Game</button>
      </div>
    </div>
  </div>
</template>

<script>

import socket from "../socket";

export default {
  name: "v-popup",
  data: function () {
    return {
      textOutput: [],
    }
  },
  created() {
    socket.on('isChancellorHitler', (text) => {
      this.textOutput.push(text);
    });
  },
  methods: {
    restartGame() {
      this.$emit('restartGame');
    }
  }
}
</script>

<style scoped lang="scss">

#id {
  font-family: "Monotype Corsiva";
  font-size: 30px;
  font-weight: bold;
}

.popup_wrapper {
  background: rgba(64, 64, 64, .6);
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  right: 0;
  left: 0;
  top: 0;
  bottom: 0;
  z-index: 7;
  transition: all 1s ease;
}
.v-popup {
  height: 540px;
  width: 990px;
  box-shadow: 0 0 17px 0 #E7E7E7;
  border-radius: 10px;
  background-image: url("../assets/fascist_ending.png");
  background-repeat: no-repeat;
  &__header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  &__content {
    display: flex;
    justify-content: center;
    align-items: center;
  }
}

.restartButton {
  font-family: "Monotype Corsiva";
  font-size: 30px;
  font-weight: bold;
  background-color: #FFEBCD;
  border-radius: 5px;
  position: relative;
  top: 100px;
  left: 50%;
  transform: translate(-50%, 0);
}

.restartButton:hover {
  background-color: #e36247;
}
</style>