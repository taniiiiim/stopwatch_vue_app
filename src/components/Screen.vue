<template>
  <div class="screen">
    <div class="function">
      <h1><span id="fnc">{{functionName}}</span></h1>
    </div>
    <div class="time">
      <h2>{{hour}}:{{min}}:{{sec}}.<span id="tenms">{{tenms}}</span></h2>
    </div>
  </div>
</template>

<script>
import { SECOND, MINUTE, HOUR } from '../constants.js'

export default {
  name: 'screen',
  props: {
    time: {
      type: Number,
      default: 0,
      required: true
    },
    isRunning: {
      type: Boolean,
      default: false,
      required: true
    },
    functionName: {
      type: String,
      default: "Stopwatch",
      required: true
    }
  },
  computed: {
    hour: function() {
      return ("0" + Math.floor(this.time / HOUR)).slice(-2);
    },
    min: function() {
      return ("0" + Math.floor((this.time % HOUR) / MINUTE)).slice(-2);
    },
    sec: function() {
      return ("0" + Math.floor(((this.time % HOUR) % MINUTE) / SECOND)).slice(-2);
    },
    tenms: function() {
      return ("0" + Math.floor(((this.time % HOUR) % MINUTE) % SECOND)).slice(-2);
    }
  }
}

</script>

<style lang="scss">
.function {
  h1 {
    display: inline-block;
    width: 240px;
    background: #dd6867;
    font-size: 40px;
    line-height: 40px;
    margin-top: 60px;
    margin-bottom: 0;
    padding: 10px;
    border-radius: 2px;
    color: #fff;
  }
}

.time {
  h2 {
    display: inline-block;
    width: 400px;
    font-size: 80px;
    line-height: 80px;
    margin-top: 60px;
    margin-right: 40px;
    background: #fafafb;
    padding: 0;

    #tenms {
      font-size: 50px;
      line-height: 50px;
    }
  }
}
</style>
