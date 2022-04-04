<template>
  <div class="row">
    <div class="col">
      <div class="select-box" id="work-select-box">
        <h2 class="setup-title">Work</h2>
        <input class="time-input" type="number" v-model.number="work_time" placeholder="25">
      </div>
      <div class="select-box" id="break-select-box">
        <h2 class="setup-title">Break</h2>
        <input class="time-input" type="number" v-model.number="break_time" placeholder="5">
      </div>
    </div>
    <button id="setup-btn" @click="start_working">Start working</button>
  </div>
</template>


<script>
export default {

    name: 'SetupScreen',

    data() {
        return {
            work_time: null,
            break_time: null,
        }
    },

    methods: {
        start_working() {
          if ((this.break_time !== null && this.work_time !== null) && !(this.break_time <= 0 || this.work_time <= 0)) {
            this.$emit('start_working', this.work_time, this.break_time)
          }
        }
    }

}
</script>


<style>

.row {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
}

.col {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  width: 100%;
}

.select-box {
  width: 50%;
  padding: 10% 1rem 10% 1rem;
  margin: 1rem;
  background-color: #222;
  border-radius: 1rem;
  text-align: center;
}

.setup-title {
  text-align: center;
  color: white;
  font-size: 1.25rem;
  font-weight: lighter;
}

.time-input {
  background-color: #111;
  max-width: 100%;
  border: none;
  border-radius: .5rem;
  color: white;
  text-align: center;
  font-size: 1rem;
  font-weight: bold;
  margin: 1rem 0 1rem 0;
  appearance: textfield;
}

.time-input::-webkit-outer-spin-button, .time-input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

#setup-btn {
  width: 100%;
  text-align: center;
  margin: 1rem 1rem 0 1rem;
  padding: .75rem 1rem;
  border: none;
  border-radius: 1rem;
  background-color: #111;
  color: white;
}

#setup-btn:hover {
  cursor: pointer;
  background-color: black;
}

</style>