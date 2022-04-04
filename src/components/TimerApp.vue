<template>
        <div id="timer">
            <div id="mode-text" :style="mode_color">{{ mode_text }}</div>
            <div id="time-text">{{ formatted_time }}</div>
            <button id="toggle-timer-btn" @click="pause_pressed">||</button>
        </div>
</template>

<script>
export default {
    name: 'TimerApp',

    props: {
        time_left: {
            type: Number,
            required: true
        },
        mode: {
            type: Boolean,
            required: true
        }
    },

    computed: {
        formatted_time() {
            let time_left = this.time_left

            let minutes = Math.floor(time_left / 60) // floor = abrunden
            let seconds = time_left % 60 // Rest = Sekunden

            if (seconds < 10) {
                seconds = `0${seconds}` // dass ab weniger als 10 zb 09 gezeigt wird
            }

            return `${minutes}:${seconds}`

        },
        mode_text() { // work and break text at the top
            if (this.mode) {
                return 'Work'
            }
            else {
                return 'Break'
            }
        },
        mode_color() { // color for the work/break text at the top
            if (this.mode) {
                return {'color': '#8a7fca'}
            }
            else {
                return {'color': '#d15555'}
            }
        }
    },

    methods: {
        pause_pressed() {
            this.$emit('pause_pressed')
        }
    }
}
</script>

<style>

#timer {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    background-color: #222;
    border-radius: 1rem;
    height: calc(100vh - 8rem);
}

#mode-text {
    font-size: 5rem;
    font-weight: bold;
    margin-top: 3rem;
}

#time-text {
    font-size: 4.5rem;
}

#toggle-timer-btn {
    text-align: center;
    padding: .75rem 1rem;
    margin-bottom: 5rem;
    border: none;
    border-radius: 1rem;
    background-color: #111;
    color: white;
    white-space: nowrap;
}

#toggle-timer-btn:hover {
    cursor: pointer;
    background-color: black;
}

</style>