<template>
  <div class="home">
    <div ref="outer" class="outer">
      <div :style="transformStyle" ref="container" class="container-row">
        <div class="card"></div>
        <div class="card"></div>
        <div class="card"></div>
        <div class="card"></div>
        <div class="card"></div>
        <div class="card"></div>
        <div class="card"></div>
      </div>
    </div>
    <button style="width: 100px; height: 30px" v-on:click="tapNext">next</button>
  </div>
</template>

<script>
import animations from 'create-keyframe-animation'
export default {
  name: 'Home',
  data: function () {
    return {
      startX: 0,
      startTrans: 0,
      currentOffset: 0,
      currentIndex: 0,
      animationOffset: 0,
      shouldSlide: false,
      domOffset: 0,
      run: {}
    }
  },
  computed: {
    transformStyle: function () {
      return {
        transform: 'translateX(' + this.domOffset + 'px)'
        // opacity: 1
      }
    }
  },
  methods: {
    touchStart: function (ev) {
      this.startX = ev.changedTouches[0].pageX;
      let transX = window.getComputedStyle(this.$refs.container, null).transform
      if (transX !== 'none') {
        this.startTrans = parseFloat(transX.split(',')[4])
        console.log(this.startTrans)
      }
    },
    touchMove: function (ev) {
      const currentX = ev.changedTouches[0].pageX;
      const offset = currentX - this.startX;
      console.log(offset + this.startTrans)
      const result = offset + this.startTrans;
      const maxOffset = 2220 - 400 + 100
      if (result <= 0 && result >= -maxOffset) {
        this.domOffset = offset + this.startTrans;
      }
    },
    touchEnd: function () {
      // 计算当前位置
      let extra = Math.abs(this.domOffset % 320)
      console.log("extra " + extra)
      let current = 0
      if (extra === 0) {
        return
      }
      if (extra >= 160) {
        current = this.domOffset
        current -= (320 - extra)
        this.currentOffset = current
      } else {
        current = this.domOffset
        current += (320 - extra)
        this.currentOffset = current
      }
      console.log('end >>> ' + this.currentOffset + "  " + this.domOffset)
      animations.registerAnimation({
        name: 'move',
        // the actual array of animation changes
        animation: [
          [current,0],
          [this.currentOffset,0]
        ],
        // optional presets for when actually running the animation
        presets: {
          duration: 0.2,
          easing: 'linear'
        }
      })
      let that = this;
      animations.runAnimation(this.$refs.container, 'move', function () {
        // callback gets called when its done
        that.domOffset = that.currentOffset;
        animations.unregisterAnimation('move')
      })
    },
    tapNext: function () {
      let index = this.currentIndex;
      if (index + 1 >= 7) {
        index = 0;
      } else {
        index += 1;
      }
      this.currentIndex = index;
      const lastOffset = this.currentOffset;
      this.currentOffset = -index * 320
      console.log(lastOffset)
      console.log(this.currentOffset)
      animations.registerAnimation({
        name: 'move',
        // the actual array of animation changes
        animation: [
          [lastOffset,0],
          [this.currentOffset,0]
        ],
        // optional presets for when actually running the animation
        presets: {
          duration: 250,
          easing: 'ease-in-out'
        }
      })
      let that = this;
      animations.runAnimation(this.$refs.container, 'move', function () {
        // callback gets called when its done
        that.domOffset = that.currentOffset;
        animations.unregisterAnimation('move')
      })
    },
  },
  mounted() {
    this.$refs.outer.addEventListener('touchstart', this.touchStart)
    this.$refs.outer.addEventListener('touchmove', this.touchMove)
    this.$refs.outer.addEventListener('touchend', this.touchEnd)
  }
}
</script>

<style scoped lang="scss">

@keyframes slideRow {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(0);
  }
}

.slide-row {
  animation-duration: 1s;
  animation: slideRow;
}

.outer {
  width: 400px;
  height: 800px;
  background: #2c3e50;
  margin-left: 500px;
  overflow: hidden;

}

.container-row {
  display: flex;
  padding: 30px 50px 30px 50px;
  width: 2220px;
}

.card {
  width: 300px;
  height: 400px;
  background: #42b983;
  flex-shrink: 0;
  flex-basis: 300px;
  margin-right: 20px;
}
</style>
