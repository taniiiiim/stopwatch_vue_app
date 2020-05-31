<template>
  <div class="layer">
    <div class="window">
      <h1 class="message">Time Up!</h1>
      <div class="close-btn">
        <input class="btn" type="button" value="Close" @click='handleCloseTimeOveredModal'/>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'time-overed-modal',
  methods: {
    handleCloseTimeOveredModal: function() {
      this.$emit('close-time-overed-modal');
    }
  }
}
</script>

<style lang="scss">
.layer {
  position: absolute;
  z-index: 5;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.35);
}

.window {
  display: block;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  height: 500px;
  width: 500px;
  box-sizing: border-box;
  background: #fff;
  border: 1px solid #fafafb;
  border-radius: 5px;
  box-shadow: 0 2px 8px 0 rgba(0,0,0,0.2);

  h1 {
    margin-top: 100px;
    color: #dd6867;
    font-size: 60px;
  }
}

.close-btn {
  margin-top: 100px;
}

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

</style>
