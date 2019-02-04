<template>
    <div class="timer-wrapper" @keydown.space="clearTimer" @keyup.space="startTimer" tabindex="0">
        {{ title }}
      <div class="timer-shower">
          {{ prettyTime }}
      </div>
    </div>
</template>
<script>
export default {
  name: 'Timer',
  data: function () {
    return {
      prettyTime: '00:00.000',
      time: 0,
      handle: undefined,
      timerRunning: false
    }
  },
  props: {
    title: String
  },
  methods: {
    startTimer (event) {
      console.log('Start Timer')
      if (!this.handle && !this.timerRunning) {
        this.timerRunning = true
        var start = Date.now()
        this.handle = setInterval(() => {
          var delta = Date.now() - start // milliseconds elapsed since start
          this.time = delta

          var milliseconds = parseInt(this.time % 1000)
          var seconds = parseInt((this.time / 1000) % 60)
          var minutes = parseInt((this.time / (1000 * 60)) % 60)

          milliseconds = (milliseconds < 10) ? '00' + milliseconds : ((milliseconds < 100) ? '0' + milliseconds : milliseconds)
          seconds = (seconds < 10) ? '0' + seconds : seconds
          minutes = (minutes < 10) ? '0' + minutes : minutes
          this.prettyTime = minutes + ':' + seconds + '.' + milliseconds
        }, 1)
      } else if (!this.handle && this.timerRunning) {
        this.timerRunning = false
      }
    },
    clearTimer (event) {
      console.log('Clear Timer')
      if (this.handle) {
        this.handle = clearInterval(this.handle)
      } else {
        this.prettyTime = '00:00.000'
        this.time = 0
        // change color or something
      }
    }
  }
}
</script>
<style scoped>
.timer-wrapper {
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif
}

.timer-shower {
  font-family: 'Courier New', Courier, monospace
}
</style>
