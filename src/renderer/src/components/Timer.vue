<script setup>
import { ref } from 'vue'

const remainingTime = ref('00:00')
const checkMark = ref('')
const reps = ref(0)

const MINUTES = 60

const timer_label = ref('Timer')

const work_time = ref(25)
const short_break = ref(5)
const long_break = ref(20)

let intervalId

const countDown = (timer) => {
  intervalId = setInterval(() => {
    let minutes = Math.floor(timer / 60)
    let seconds = timer % 60
    if (seconds < 10) {
      seconds = '0' + `${seconds}`
    }
    if (minutes < 10) {
      minutes = '0' + `${minutes}`
    }
    remainingTime.value = `${minutes}:${seconds}`

    if (timer > 0) {
      timer--
    } else {
      clearInterval(intervalId)
      startTimer()

      // let work_session = Math.floor(reps.value/2)
      if (reps.value % 2 == 0) [(checkMark.value += `✔`)]
    }
  }, 1000)
}

const startTimer = () => {
  reps.value++
  if (reps.value % 8 == 0) {
    timer_label.value = 'Break'
    countDown(long_break.value * MINUTES)
  } else if (reps.value % 2 == 0) {
    timer_label.value = 'Break'
    countDown(short_break.value * MINUTES)
  } else {
    timer_label.value = 'Work'
    countDown(work_time.value * MINUTES)
  }
}

const resetTimer = () => {
  if (intervalId !== null) {
    clearInterval(intervalId)
  }
  reps.value = 0
  timer_label.value = 'Timer'
  remainingTime.value = `00:00`
  checkMark.value = ''
}
</script>

<template>
  <div class="flex">
    <h1>Configure Your Pomodoro Timer</h1>
    <div class="timer-config">
      <div class="input-group">
        <label for="work">Work Time</label>
        <input type="number" v-model="work_time" />
      </div>
      <div class="input-group">
        <label for="short_break">Short Break</label>
        <input type="number" id="short_break" v-model="short_break" />
      </div>
      <div class="input-group">
        <label for="long_break">Long Break</label>
        <input type="number" id="long_break" v-model="long_break" />
      </div>
    </div>
    <div class="circle-container">
      <div class="semicircle"></div>
      <div class="outermost-circle">
        <h4>{{ timer_label }}</h4>
        <span>{{ remainingTime }}</span>
      </div>
    </div>

    <div class="button-wrapper">
      <button @click="startTimer">Start</button>
      <button @click="resetTimer">Reset</button>
    </div>
    <span>
      {{ checkMark }}
    </span>
  </div>
</template>

<style scoped>
.flex > h1 {
  margin-top: 10pt;
  font-size: 15pt;
}
.flex {
  display: flex;
  flex-direction: column;
  gap: 30pt;
  text-align: center;
  font-size: 24pt;
  align-items: center;
}

.button-wrapper {
  display: flex;
  gap: 200pt;
}

.button-wrapper button {
  padding: 10pt 10pt;
  width: 100pt;
  font-size: 24pt;
  border-radius: 20pt;
  border-color: #b6bbc4;
}

.timer-config {
  display: flex;
  gap: 20pt;
  /* flex-direction: column; */
}
.timer-config > .input-group > label {
  font-size: 15pt;
}
.timer-config > .input-group {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 20pt;
}
.timer-config > .input-group > input {
  height: 30pt;
  padding: 7pt;
  border-radius: 20pt;
  width: 20%;
  color: white;
  font-size: 20pt;
  border: 1px solid #f0ece5;
}
.timer-config > .input-group > input:focus {
  background-color: #f0ece5;
  outline: none;
  color: #161a30;
}
/* button:hover{
    background-color: white;
} */

.circle-container {
  width: 200pt;
  height: 200pt;
  background-color: #f0ece5;
  border-radius: 50%;
  position: relative;
  overflow: hidden;
}

.semicircle {
  width: 100%;
  height: 100%;
  position: absolute;
  animation: rotate 5s linear infinite;
  background: conic-gradient(
    from 0deg,
    #ff0000 0%,
    /* red */ #ff6666 25%,
    /* lighter shade of red */ #0000ff 50%,
    /* blue */ #6666ff 75%,
    /* lighter shade of blue */ #ff0000 100% /* back to red */
  );
}

@keyframes rotate {
  to {
    transform: rotate(360deg);
  }
}

.outermost-circle {
  width: 180pt;
  height: 180pt;
  background-color: #161a30;
  border-radius: 50%;
  z-index: 5;
  /* display: none; */
  top: 50%;
  left: 50%;
  position: absolute;
  transform: translate(-50%, -50%);
  display: flex;
  flex-direction: column;
  gap: 10pt;
  justify-content: center;
  align-items: center;
}

.outermost-circle > span {
  font-size: 25pt;
}
</style>
