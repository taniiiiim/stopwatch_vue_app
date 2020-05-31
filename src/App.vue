<template>
  <main>
    <div class="box">
      <div class="watch">
        <Screen
          :time='time'
          :is-running='isRunning'
          :function-name='functionName'
        ></Screen>
        <div class="btns">
          <LeftButton
            :is-running='isRunning'
            :time-overed='timeOvered'
            @process='process'
          ></LeftButton>
          <RightButton
            :is-running='isRunning'
            @reset='reset'
          ></RightButton >
        </div>
        <BottomButton
          :is-running='isRunning'
          :time-overed='timeOvered'
          @toggle='toggle'
        ></BottomButton>
      </div>
      <div class="bar" v-if='functionName==="Timer"'>
        <div class="hour">
          <span>Hour</span>
          <input type="range"
            class="initial-time-change-bar"
            max="24"
            min="0"
            :value="initialTimeHour"
            @input="initialTimeHourChange"
            :disabled="isRunning"
          />
        </div>
        <div class="min">
          <span>Min</span>
          <input type="range"
            class="initial-time-change-bar"
            max="59"
            min="0"
            :value="initialTimeMin"
            @input="initialTimeMinChange"
            :disabled="isRunning"
          />
        </div>
        <div class="sec">
          <span>Sec</span>
          <input type="range"
            class="initial-time-change-bar"
            max="59"
            min="0"
            :value="initialTimeSec"
            @input="initialTimeSecChange"
            :disabled="isRunning"
          />
        </div>
      </div>
    </div>
    <transition>
      <TimeOveredModal
        v-if="shouldOpenTimeOveredModal"
        @close-time-overed-modal="closeTimeOveredModal"
      ></TimeOveredModal>
    </transition>
  </main>
</template>

<script>
let timerId

import Screen from './components/Screen'
import LeftButton from './components/LeftButton'
import RightButton from './components/RightButton'
import BottomButton from './components/BottomButton'
import TimeOveredModal from './components/TimeOveredModal'
import { SECOND, MINUTE, HOUR, MAX_TIME, FUNCTION_NAMES } from './constants.js'

export default {
  name: 'app',
  components: {
    Screen,
    LeftButton,
    RightButton,
    BottomButton,
    TimeOveredModal
  },
  data: function() {
    return {
      initialTime: 0,
      time: 0,
      isRunning: false,
      functionName: "Stopwatch",
      shouldOpenTimeOveredModal: true
    }
  },
  computed: {
    timeOvered: function() {
      if (this.functionName === "Stopwatch") {
        return this.time === MAX_TIME;
      } else if (this.functionName === "Timer") {
        return this.time === 0;
      }
    },
    initialTimeHour: function() {
      return Math.floor(this.initialTime / HOUR);
    },
    initialTimeMin: function() {
      return Math.floor((this.initialTime % HOUR) / MINUTE);
    },
    initialTimeSec: function() {
      return Math.floor(((this.initialTime % HOUR) % MINUTE) / SECOND);
    }
  },
  methods: {
    process: function() {
      this.isRunning = !this.isRunning;
      if (this.isRunning) {
        timerId = setInterval(() => {
          if (this.functionName === "Stopwatch") {
            this.time += 1;
          } else if (this.functionName === "Timer") {
            this.time -= 1;
          }
          if (this.timeOvered) {
            this.isRunning = !this.isRunning;
            clearInterval(timerId);
            this.shouldOpenTimeOveredModal = true;
          }
        }, 10);
      } else {
        clearInterval(timerId);
      }
    },
    reset: function() {
      if (this.isRunning) {
        this.isRunning = !this.isRunning;
        clearInterval(timerId);
      } else {
        this.time = this.initialTime;
      }
    },
    toggle: function() {
      let toggle_index = (FUNCTION_NAMES.findIndex(elem => elem === this.functionName) + 1) % FUNCTION_NAMES.length;
      this.functionName = FUNCTION_NAMES[toggle_index];
      switch (this.functionName) {
        case "Stopwatch":
          this.initialTime = 0;
          break;
        case "Timer":
          this.initialTime = 3 * MINUTE;
          break;
        default:
          this.initialTime = 0;
          break;
      }
      this.time = this.initialTime;
    },
    initialTimeHourChange: function(e) {
      this.initialTime = parseInt(e.target.value) * HOUR + Math.floor(this.initialTime % HOUR);
      this.time = this.initialTime;
    },
    initialTimeMinChange: function(e) {
      this.initialTime = Math.floor(this.initialTime / HOUR) * HOUR + parseInt(e.target.value) * MINUTE + Math.floor(this.initialTime % MINUTE);
      this.time = this.initialTime;
    },
    initialTimeSecChange: function(e) {
      this.initialTime =  Math.floor(this.initialTime / MINUTE) * MINUTE + parseInt(e.target.value) * SECOND;
      this.time = this.initialTime;
    },
    closeTimeOveredModal: function() {
      this.shouldOpenTimeOveredModal = false;
    }
  }
}
</script>

<style lang="scss">
main {
  font-family: serif;
  text-align: center;
  color: #2D2D2D;
}

.box {
  z-index: 1;
  position: relative;
  height: 700px;
  width: 600px;
  box-sizing:border-box;
  margin: auto;
  margin-top: 0px;
}

.watch {
  z-index: 2;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 100px;
  right: 0;
  border: 1px solid #fafafb;
  border-radius: 300px;
  background: #fafafb;
  box-shadow: 0 2px 8px 0 rgba(0,0,0,0.2);
}

.btns {
  z-index: 3;
  position: absolute;
  top: 280px;
  left: 0;
  right: 0;
  height: 280px;
}

.btn {
  position: relative;
  z-index: 4;
}

.bar {
  z-index: 2;
  position: absolute;
  height: 100px;
  width: 600px;
  top: 600px;
  bottom: 0px;
}

.hour, .min, .sec {
  z-index: 3;
  position: relative;
  height: 40px;
  line-height: 25px;
  font-size: 25px;
  font-weight: bold;

  span {
    z-index: 4;
    position: absolute;
    top: 15px;
    left: 10px;
  }
}

.initial-time-change-bar {
  z-index: 3;
  -webkit-appearance: none;
  appearance: none;
  height: 20px;
  width: 500px;
  background: #fafafb;
  border: 1px solid #fafafb;
  border-radius: 10px;
  box-shadow: 0 2px 8px 0 rgba(0,0,0,0.2);
  margin-top: 20px;
  margin-left: 80px;
  cursor: pointer;

  &:focus {
    outline: none;
  }

  &::-webkit-slider-thumb {
    -webkit-appearance: none;
    height: 24px;
    width: 24px;
    background: #dd6867;
    border: 1px solid #dd6867;
    border-radius: 12px;
  }

  &:active::-webkit-slider-thumb {
    height: 36px;
    width: 36px;
    border-radius: 18px;
    box-shadow: 0 2px 8px 0 rgba(221,104,103,0.2);
  }

  &:disabled::-webkit-slider-thumb {
    border-color: rgb(170, 170, 170);
    background: rgb(170, 170, 170);
    box-shadow: 0 2px 8px 0 rgba(0,0,0,0.2);
  }
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.2s ease-out;
}

.v-enter {
  opacity: 0;
}

.v-enter-to {
  opacity: 1;
}

.v-leave {
  opacity: 1;
}

.v-leave-to {
  opacity: 0;
}
</style>
