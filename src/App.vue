<template>
  <main id="app">
    <section class="logo-wrapper">
      <div class="bg-logo">
        <img class="img-logo" src="/imgs/LOGO_POOLTORIUM.png" alt="pooltorium">
      </div>
    </section>
    <section class="time">
      <div class="time-wrapper">
        <!-- <span refs="timer" class="countdown" :class="{ 'text-danger': isWillTimeOut }">{{ timeFormat }}</span> -->
        <span class="time-digit" :class="{ 'text-danger': isWillTimeOut }">{{ digitTen }}</span>
        <span class="time-digit" :class="{ 'text-danger': isWillTimeOut }">{{ digitOne }}</span>
        <!-- <span class="time-digit" :class="{ 'text-danger': isWillTimeOut }">.0</span>
        <span class="time-digit" :class="{ 'text-danger': isWillTimeOut }">0</span> -->
      </div>
      <!-- <span class="sec">Sec.</span> -->
    </section>
    <section class="control">
      <span @click="reset" refs="resetBtn" class="control-reset" :class="{ 'text-white': isRun }">reset</span>
      <span v-if="!isRun" @click="start" refs="startBtn" class="control-start" :class="{ 'text-gray': isTimeout, 'hide': isTimeout }">start</span>
      <!-- <span v-if="isStop && time !== 30" @click="start" refs="startBtn" class="control-start">resume</span> -->
      <span v-if="isRun" @click="stop" refs="stopBtn" class="control-start" :class="{ 'text-white': isRun }">stop</span>
    </section>
  </main>
</template>

<script>
let interval
export default {
  name: 'app',
  data () {
    return {
      time: 30,
      isWillTimeOut: false,
      isReset: false,
      isTimeout: false,
      isRun: false,
      digitTen: 3,
      digitOne: 0
    }
  },

  watch: {
    time: function(val) {
      if (val === 0) {
        this.isRun = false
        this.isTimeout = true
        this.digitOne = 0
      } else if (val < 10) {
        this.digitTen = 0
        this.digitOne = val
      } else if (val < 11) {
        this.isWillTimeOut = true
        this.digitOne = this.digitTwo = val.toString().substr(1, 1)
        this.digitTen = 1
      } else if (val < 20) {
        this.digitTen = 1
        this.digitOne = val.toString().substr(1, 1)
      } else if (val < 30) {
        this.digitTen = 2
        this.digitOne = val.toString().substr(1, 1)
      }
      this.time = val
    }
  },

  computed: {
    timeFormat () {
      return this.time.toFixed(2)
    }
  },

  mounted () {
    const self = this
    window.addEventListener('keydown', e => {
      this.isRun = !this.isRun
      if (e.code === 'Space') {
        if (this.isRun) {
          this.isReset = false
          self.countdown()
          return
        }

        clearInterval(interval)
      }
      if (e.code === 'ControlLeft') {
        self.reset()
      }

      if (!this.isRun) {
        clearInterval(interval)
        return
      }
    })
  },
  methods: {
    countdown () {
      interval = setInterval(() => {
        if (this.time === 0) return
        this.time = this.time - 1
      }, 1000)
    },

    start () {

    },

    stop () {

    },

    reset () {
      this.isReset = true
      this.isRun = false
      this.isWillTimeOut = false
      this.isTimeout = false
      this.time = 30
      this.digitTen = 3
      this.digitOne = 0
      clearInterval(interval)
    },

    formatPrice(value) {
      let val = (value/1).toFixed(2).replace('.', '.')
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
    }
  }
}
</script>
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

@font-face {
  font-family: NexaBold;
  src: url("./assets/fonts/Nexa_Bold.otf") format("opentype");
  font-weight: bold;
}

body {
  font-family: NexaBold;
  height: 100%;
  color: #fff;
}

main {
  background: url("../public/imgs/bg.png") no-repeat;
  background-size: cover;
  overflow: hidden;
}

.logo-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
}

.bg-logo {
  margin-top: 20px;
  height: 200px;
  width: 400px;
}

.img-logo {
  width: 100%;
  height: auto;
}

.time {
  margin-top: 200px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row;
  /* padding-left: 70px;
  padding-right: 20px; */
}

.time-wrapper {
  
}

.time-digit {
  font-size: 300px;
}

.countdown {
  font-size: 350px;
  max-width: 815px;
}

.sec {
  font-size: 60px;
  color: #b7b7b7;
  text-transform: uppercase;
  align-self: center;
}

.control {
  display: flex;
  justify-content: space-around;
}

.control-reset {
  margin-right: auto;
  font-size: 60px;
  text-transform: uppercase;
  margin-left: 40px;
}

.control-start {
  margin-left: auto;
  font-size: 60px;
  text-transform: uppercase;
  margin-right: 40px;
}

.hide {
  display: none;
}

.text-danger {
  color: red;
}

.text-white {
  color: #fff;
}

.text-gray {
  color: #B7B7B7;
}
</style>
