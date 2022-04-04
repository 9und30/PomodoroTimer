<template>
  <div id="title">
    <span class="title" id="title-1">Pomodoro</span>
    <span class="title" id="title-2">Timer</span>
  </div>
  <div id="content">
    <SetupScreen v-if="!enabled" @start_working="set_time"/>
    <TimerApp v-else :time_left="time_left" :mode="is_working" @pause_pressed="toggle_timer(); change_mode()"/>
  </div>
</template>

<script>
import TimerApp from './components/TimerApp.vue'
import SetupScreen from './components/SetupScreen.vue'

export default {

  name: 'App',

  components: {
    TimerApp,
    SetupScreen
  },

  data() {
    return {
      enabled: false,
      work_time: null,
      break_time: null,
      is_working: true,
      paused: false,
      timer_up: false,
      alarm: new Audio(require('./assets/timer_up.mp3')),
      audio_playing: false,
      time_passed: 0,
      should_run: true,
    }
  },

  computed: {
    time_left() {
      if (this.is_working) {
        return this.work_time - this.time_passed
      }
      else {
        return this.break_time - this.time_passed
      }
    }
  },

  methods: {

    set_time(work_time, break_time) {
      this.work_time = work_time * 60
      this.break_time = break_time * 60
      this.enabled = true
      this.run_timer()
    },

    run_timer() {
      const t = setInterval(()=>{
        if (!this.paused) {
          if (this.is_working) { // if working and time greater than 0, run, otherwise change mode
            if (!(this.time_passed >= this.work_time)) {
            this.time_passed += 1
            }
            else {
              this.timer_up = true
              this.play_sound()
            }
          }
          else { // same thing here just for break
            if (!(this.time_passed >= this.break_time)) {
              this.time_passed += 1
            }
            else {
              this.timer_up = true
              this.play_sound()
            }
          }
        }
      }, 1000) // every 1000ms
      if (!this.should_run) {
          clearInterval(t)
        }
    },

    toggle_timer() {
      if (!this.timer_up) {
        this.paused = !this.paused
      }
    },

    play_sound() {
      this.alarm.addEventListener('ended', () => {
        this.audio_playing = false // replay if sound is finished
      })
      if (this.timer_up && !this.audio_playing) { // make sure the audio is not playing already
        this.alarm.play()
        this.audio_playing = true
      }
    },

    break_audio() {
      this.alarm.pause()
      this.alarm.currentTime = 0
      this.audio_playing = false
    },

    change_mode() {
      if (this.timer_up) {
        this.is_working = !this.is_working
        this.time_passed = 0
        this.break_audio()
        this.timer_up = false
      }
    },

  }

}
</script>

<style>
*, *::before, *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body {
  background: #333;
  font-family: 'Poppins';
  width: 100%;
  height: 100%;
}

#app {
  padding: 2rem 3rem;
  color: white;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

#title {
  font-size: 2rem;
  font-weight: bold;
}

#title-1 {
  color: #d15555;
}

#title-2 {
  color: #8a7fca;
}

#content {
  margin-top: auto;
  margin-bottom: auto;
  width: 100%;
}


</style>
