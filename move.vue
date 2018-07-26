<template>
  <div @mousedown="downBlock" class="move_container">
    <div v-if="!success" class="hint">请按住滑块，拖动到最右边</div>
    <div v-else class="successMsg">验证通过</div>
    <div :style="{width:left}" class="left_block"></div>
    <div :style="{left:left}" :class="['move_block', success ? 'successBg' : 'movebg']"></div>
  </div>
</template>
<script>
export default {
  name: 'move',
  data () {
    return {
      // 滑块位置
      left: 0,
      // 点击位置
      startX: 0,
      // 滑块起始位置
      startLeft: 0,
      // 滑块结束位置
      endLeft: 450,
      // 滑动标识
      flag: false,
      // 验证通过
      success: false
    };
  },
  methods: {
    downBlock (e) {
      let that = this;
      if (!this.success) {
        this.flag = true;
        this.startX = e.clientX;
      }
      document.onmousemove = function (e) {
        if (!that.success && that.flag) {
          if (parseInt(that.left) < that.startLeft) {
            that.left = that.startLeft;
            that.flag = false;
          } else if (parseInt(that.left) >= that.endLeft) {
            that.left = that.endLeft + 'px';
            that.success = true;
            that.$emit('pass', 'pass');
            that.flag = false;
          } else {
            that.left = e.clientX - that.startX + 'px';
          }
        }
      };
      document.onmouseup = function (e) {
        if (!that.success) {
          that.flag = false;
          that.left = that.startLeft;
        }
      };
    }
  }
};
</script>
<style lang="less" scoped>
.move_container {
  width: 520px;
  height: 46px;
  position: relative;
  overflow: hidden;
  background: #e8e8e8;
  color: #fff;
  font-size: 14px;
  text-align: center;
  line-height: 46px;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  -khtml-user-select: none;
  user-select: none;
  cursor: pointer;
  .successMsg {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-70%, -50%);
    color: #fff;
    font-size: 14px;
  }
  .hint {
    position: absolute;
    top: 50%;
    left: 0;
    transform: translate(0, -50%);
    height: 20px;
    width: 100%;
    text-align: center;
    line-height: 20px;
    background: -webkit-gradient(
      linear,
      left top,
      right top,
      color-stop(0, #4d4d4d),
      color-stop(0.4, #4d4d4d),
      color-stop(0.5, #fff),
      color-stop(0.6, #4d4d4d),
      color-stop(1, #4d4d4d)
    );
    -webkit-text-fill-color: transparent;
    -webkit-background-clip: text;
    animation: slidetounlock 3s infinite;
  }
  .move_block {
    width: 70px;
    height: 100%;
    position: absolute;
    top: 0;
    transition: left 0.1s ease-out;
    border: 1px solid #cccccc;
    cursor: move;
  }
  .movebg {
    background: #fff url(/static/images/move.png) no-repeat center center / 16px;
  }
  .successBg {
    background: #fff url(/static/images/success.png) no-repeat center center /
      16px;
  }
  .left_block {
    height: 100%;
    background: #7ac23c;
  }
}

@keyframes slidetounlock {
  0% {
    background-position: -200px 0;
  }

  100% {
    background-position: 200px 0;
  }
}
</style>
