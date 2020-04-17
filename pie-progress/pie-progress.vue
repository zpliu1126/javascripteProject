<template>
  <div class="vue-pie-progress">
    <p class="vue-pie-progress-number">{{showprogress}}%</p>
    <svg
      style="transform: rotate(-90deg);text-align:center"
      :width="width"
      :height="width"
      xmlns="http://www.w3.org/2000/svg"
    >
      <circle
        :r="(width-radius)/2"
        :cy="width/2"
        :cx="width/2"
        :stroke-width="radius"
        :stroke="backgroundColor"
        fill="none"
      />
      <circle
        ref="$bar"
        :r="(width-radius)/2"
        :cy="width/2"
        :cx="width/2"
        :stroke="barColor"
        :stroke-width="radius"
        :stroke-linecap="isRound ? 'round' : 'square'"
        :stroke-dasharray="(width-radius)*3.14"
        :fill="'none'"
      />
    </svg>
  </div>
</template>

<script>
import TWEEN from '@tweenjs/tween.js'
export default {
  name: "circleProgress",
  props: {
    width: [Number, String], // 图片的大小
    radius: [Number, String], // 进度条厚度
    progress: [Number, String], // 进度条百分比
    barColor: {
      type: String,
      default: '#54a0ff'
    }, // 进度条颜色
    backgroundColor: {
      type: String,
      default: 'rgba(206, 206, 206, 0.4)'
    }, // 背景颜色
    isRound: { // 是否是圆形画笔
      type: Boolean,
      default: true,
    },
    duration: { // 整个动画时长
      type: [String, Number],
      default: 3000,
    },
  },
  data () {
    return {
      showprogress: 0
    }
  },
  computed: {

  },
  beforeDestroy () {

  },
  methods: {
    progressAnimate () {
      this.$refs.$bar.style.strokeDashoffset = (this.width - this.radius) * 3.14
      this.showprogress = 0
      new TWEEN.Tween(
        {
          'strokeDashoffset': (this.width - this.radius) * 3.14,
          'showProgress': 0
        }
      ).to({
        'strokeDashoffset': (this.width - this.radius) * 3.14 * (100 - this.progress) / 100,
        'showProgress': this.progress
      }, this.duration).onUpdate((tween) => {
        this.$refs.$bar.style.strokeDashoffset = tween.strokeDashoffset
        this.showprogress = tween.showProgress.toFixed(0)
      }).start()
      this.StartAnimate()
    },
    StartAnimate () {
      function animate () {
        if (TWEEN.update()) {
          requestAnimationFrame(animate)
        }
      }
      animate()
    }
  },
  mounted () {
    this.progressAnimate()
  }
}
</script>
<style scopped>
.vue-pie-progress {
  display: flex;
  position: relative;
  text-align: center;
  align-items: center; /*垂直居中*/
  justify-content: center; /*水平居中*/
  border: 1px solid #f00;
}
.vue-pie-progress-number {
  border: 1px solid #f00;
  position: absolute;
  display: flex;
  width: 100%;
  height: 100%;
  align-items: center; /*垂直居中*/
  justify-content: center; /*水平居中*/
  margin: 0px 0px;
  font-size: 20px;
}
</style>