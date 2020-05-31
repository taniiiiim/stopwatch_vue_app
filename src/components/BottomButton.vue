<template>
  <div class="bottom">
    <input class="btn" type="button" value="Toggle" @click='handleToggle' :disabled='isRunning && !timeOvered'/>
  </div>
</template>

<script>
export default {
  name: 'bottom-button',
  props: {
    isRunning: {
      type: Boolean,
      default: false,
      required: true
    },
    timeOvered: {
      type: Boolean,
      default: false,
      required: true
    }
  },
  methods: {
    handleToggle: function() {
      this.$emit('toggle');
    }
  }
}
</script>

<style lang="scss">
.btn {
  background: #fff;
  height: 150px;
  width: 150px;
  line-height: 25px;
  border: 1px solid #fafafb;
  border-radius: 75px;
  box-shadow: 0 2px 8px 0 rgba(0,0,0,0.2);
  font-size: 25px;
  font-family: serif;
  font-weight: bold;
  cursor: pointer;

  &:hover {
    border-color: #dd6867;
    color: #dd6867;
    box-shadow: 0 2px 8px 0 rgba(221,104,103,0.2);
  }

  &:active {
    background: #dd6867;
    color: #fff;
    box-shadow: 0 2px 8px 0 rgba(221,104,103,0.2);
  }

  &:focus {
    outline: none;
  }

  &:disabled {
    border-color: #fff;
    color: rgb(170, 170, 170);
    box-shadow: 0 2px 8px 0 rgba(0,0,0,0.2);
  }
}

.bottom {
  position: absolute;
  bottom: 40px;
  left: 225px;
}
</style>
