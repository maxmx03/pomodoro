<script>
export default {
  data() {
    return {
      started: null,
      paused: null,
      rest: null,
      interval: null,
      defaultMinutes: 24,
      defaultSeconds: 59,
      cicles: 0,
      minutes: 24,
      seconds: 59,
      shortBreak: 5,
      longBreak: 15,
    }
  },
  methods: {
    increaseCicles() {
      this.cicles += 1
    },
    resetCicles() {
      this.cicles = 0
    },
    resetMinutes() {
      this.minutes = this.defaultMinutes
    },
    decreaseMinutes() {
      this.minutes -= 1
    },
    resetSeconds() {
      this.seconds = this.defaultSeconds
    },
    decreaseSeconds() {
      this.seconds -= 1
    },
    startPomodoro() {
      if (!this.started) {
        this.interval = setInterval(this.decreaseSeconds, 1000)
        this.started = true
      }
    },
    pausePomodoro() {
      if (!this.paused) {
        clearInterval(this.interval)
        this.paused = true
      }
    },
    stopPomodoro() {
      this.resetMinutes()
      this.resetSeconds()
      clearInterval(this.interval)
    },
    resetRest() {
      this.rest = null
    },
    setShortRest() {
      this.minutes = this.shortBreak
      this.resetSeconds()
      this.rest = !this.rest
    },
    setLongRest() {
      this.minutes = this.longBreak
      this.resetSeconds()
      this.rest = !this.rest
    },
  },
  computed: {
    displayTime() {
      const formatTime = (time) => (time >= 10 ? time : `0${time}`)
      const minutes = formatTime(this.minutes)
      const seconds = formatTime(this.seconds)

      return `${minutes}:${seconds}`
    },
  },
  watch: {
    cicles(cicle) {
      if (cicle > 4) {
        this.resetCicles()
      }
    },
    minutes(minute) {
      if (minute < 0 && this.cicles < 4 && !this.rest) {
        this.setShortRest()
      } else if (minute < 0 && this.cicles >= 4 && !this.rest) {
        this.setLongRest()
      } else if (minute < 0 && this.rest) {
        this.resetMinutes()
        this.resetRest()
        this.increaseCicles()
      }
    },
    seconds(second) {
      if (second < 0) {
        this.decreaseMinutes()
        this.resetSeconds()
      }
    },
    started(started) {
      if (started) {
        this.paused = false
      }
    },
    paused(paused) {
      if (paused) {
        this.started = false
      }
    },
  },
  unmounted() {
    clearInterval(this.interval)
  },
}
</script>

<template>
  <div class="pomodoro">
    <p v-if="rest && cicles >= 4">Long Break</p>
    <p v-else-if="rest && cicles < 4">short break</p>
    <p v-else>pomodoros {{ cicles }}</p>
    <h1>{{ displayTime }}</h1>
    <button @click="startPomodoro" v-if="!started">start</button>
    <button @click="pausePomodoro" v-else>pause</button>
  </div>
</template>

<style scoped lang="scss">
@use '@/assets/_colors.scss';

.pomodoro {
  width: 30rem;
  height: 20rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 2rem;
  align-items: center;
  background-color: colors.$green;
  border-radius: 10px;

  p {
    font-size: 1.2rem;
    font-weight: bold;
    color: colors.$white;
    background-color: darken(colors.$green, 30%);
    padding: 0.5rem;
  }

  h1 {
    font-size: 6rem;
    font-weight: bold;
    color: colors.$white;
  }

  button {
    font-size: 1.6rem;
    font-weight: bold;
    color: colors.$white;
    background-color: colors.$blue;
    border-style: none;
    border-radius: 10px;
    width: 10rem;
    height: 3rem;

    &:hover {
      background-color: darken(colors.$blue, 5%);
      cursor: pointer;
    }
  }
}
</style>
