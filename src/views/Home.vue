<template>
  <div class="home">
    <div class="slide-container" ref="container" @mousedown="mouseDown" @mousemove.passive="mouseMove" @mouseup="mouseUp">
      <div class="slide-wrapper" ref="wrapper">
        <div class="slide" ref="slide" v-for="item in 8" :key="item">
          <img :src="images[item - 1]">
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import img1 from '@/assets/1.jpg';
import img2 from '@/assets/2.jpg';
import img3 from '@/assets/3.jpg';
import img4 from '@/assets/4.jpg';
import img5 from '@/assets/5.jpg';
import img6 from '@/assets/6.jpg';
import img7 from '@/assets/7.jpg';
import img8 from '@/assets/8.jpg';

const math = {
  lerp: (a, b, n) => {
    return (1 - n) * a + n * b
  },
  norm: (value, min, max) => {
    return (value - min) / (max - min)
  }
}

const config = {
  height: window.innerHeight,
  width: window.innerWidth
}


export default {
  name: 'Home',
  data: function () {
    return {
      el: null,
      content: null,
      data: null,
      bounds: {
          elem: 0,
          content: 0,
          width: 0,
          max: 0,
          min: 0
      },
      state: {
        dragging: false,
      },
      rAF: null,
      parallax: null,
      images: [
        img1,
        img2,
        img3,
        img4,
        img5,
        img6,
        img7,
        img8,
      ],
      lastTranslate: 0,
    }
  },
  computed: {
    container() {
      return this.$refs.container;
    },
    wrapper() {
      return this.$refs.wrapper;
    },
    slides() {
      return this.$refs.slide;
    },
  },
  methods: {
    drag(e) {
      this.data.current = this.lastTranslate - (e.clientX - this.data.on);
      this.clamp();
    },
    run() {
      // 插值
      this.data.last = math.lerp(this.data.last, this.data.current, 0.085);
      this.data.last = Math.floor(this.data.last * 100) / 100
      const diff = this.data.current - this.data.last;
      const acc = diff / config.width;
      const velo = acc;
      const bounce = 1 - Math.abs(velo * 0.55);
      // const scale = 1 - velo * 7.5;
      this.wrapper.style.transform = `translate3d(-${this.data.last.toFixed(2)}px, 0, 0)`;
      this.$refs.slide.forEach(item => {
        item.style.transform = `scale(${bounce})`;
      })
      this.requestAnimationFrame();
    },
    clamp() {
      this.data.current = Math.min(Math.max(this.data.current, 0), this.bounds.max);
    },
    requestAnimationFrame() {
      this.rAF = requestAnimationFrame(this.run);
    },
    mouseDown(e) {
      this.state.dragging = true;
      this.data.on = e.clientX;
    },
    mouseMove(e) {
      if (!this.state.dragging) return;
      this.drag(e);
    },
    mouseUp() {
      console.log('mouse up');
      this.state.dragging = false;
      this.lastTranslate = this.data.current;1
    }
  },
  mounted() {
    this.data = {
      total: this.slides.length - 1,
      current: 0,
      last: 0,
      on: 0,
      off: 0
    }
    this.bounds.max = this.$refs.wrapper.scrollWidth - this.$refs.container.clientWidth / 2;
    this.requestAnimationFrame();
  }
}
</script>

<style scoped lang="scss">

  .home {
    background: bisque;
    width: 100%;
    height: 800px;
    .slide-container {
      width: 100%;
      height: 100%;
      background: #2c3e50;
      display: flex;
      align-items: center;
      overflow: hidden;
      .slide-wrapper {
        display: flex;
        align-items: center;
        justify-content: flex-start;
        .slide {
          width: 400px;
          height: 300px;
          flex-shrink: 0;
          margin: 10px;
          img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            user-select: none;
            -webkit-user-drag: none;
          }
        }
      }
    }
  }


</style>
