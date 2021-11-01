<template>
  <div class="wrapper">
    <div class="keys">
      <button 
        v-for="item in keys"
        :key="item.key"
        class="key" 
        ref="clickedKey"
        :class="{playing: playing && pressedKey === item.key}" 
        @click="playSound(item.key)"
      >
        <div>
          {{ item.key }}
          <span> {{ item.sound }}</span>
        </div>
      </button>
    </div>
    <div v-for="code in keys" :key="code.keyCode">
      <audio :data-key="code.keyCode" :ref="`audio${code.key}`">
        <source :src="code.src" type="audio/wav">
      </audio>
    </div>
    <audio id="myAudio">
      <source src="../../assets/sounds/clap.wav" type="audio/wav">
      Your browser does not support the audio element.
    </audio>
    <button type="button" @click="playAudio">Play</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      keys: [
        { key: "A", sound: "CLAP", keyCode: "65", src: "../../assets/sounds/clap.wav" },
        { key: "S", sound: "HIHAT", keyCode: "83", src: "../../assets/sounds/hihat.wav" },
        { key: "D", sound: "KICK", keyCode: "68", src: "../../assets/sounds/kcik.wav" },
        { key: "F", sound: "OPENHAT", keyCode: "70", src: "../../assets/sounds/openhat.wav" },
        { key: "G", sound: "BOOM", keyCode: "71", src: "../../assets/sounds/boom.wav" },
        { key: "H", sound: "RIDE", keyCode: "72", src: "../../assets/sounds/ride.wav" },
        { key: "J", sound: "SNARE", keyCode: "74", src: "../../assets/sounds/snare.wav" },
        { key: "K", sound: "TOM", keyCode: "75", src: "../../assets/sounds/clap.tom" },
        { key: "L", sound: "TINK", keyCode: "76", src: "../../assets/sounds/clap.tink" },
      ],
      playing: false,
      pressedKey: '',
      sound: null,
    }
  },
  mounted() {
    this.keySound()
  },
  methods: {
    playAudio() {
      var x = document.getElementById("myAudio"); 
      x.play()
    },
    keySound() {
      document.addEventListener("keydown", (event) => {
        this.playSound(event)
      })
    },
    async playSound(e) {
      this.pressedKey = e.key?.toUpperCase() || e
      const keyCode = await this.keys.find((k) => k.key === this.pressedKey).keyCode
      this.playing = true;
      const audio1 = document.querySelector(`audio[data-key="${keyCode}"]`)
      // const audio = this.$refs[`audio${this.pressedKey}`]
      audio1.play()
      // audio.play()
      // setTimeout(() => {
      //   this.playing = false
      //   this.pressedKey = ''
      // }, 2000)
    },
  },
}
</script>

<style scoped>
.wrapper {
  background: url('../../assets/background.jpg') no-repeat center;
  background-size: cover;
  height: 100vh;
  width: 100vw;
  overflow-y: hidden;
}
.keys {
  display: flex;
  gap: 10px;
  justify-content: center;
  padding-top: 45vh;
}
.key {
  height: 60px;
  width: 60px;
  border: 3px solid black;
  background: transparent;
  font-size: 25px;
  font-weight: bold;
  line-height: 15px;
  color: #fff;
  text-align: center;
  padding: 0;
  transition: all 50ms ease-in-out;
}
.key div {
  width: 100%;
  height: calc(100% - 13px);
  padding-top: 13px;
  background: rgba(3, 3, 3, 0.5);
}
.key span{
  display: block;
  margin-top: 8px;
  font-size: 10px;
  color: yellow;
}
.playing {
  border: 4px solid yellow;
  transform: scale(1.1);
}
</style>