<template>
  <svg :viewBox="`0 0 ${box} ${box}`" preserveAspectRatio="none">
    <circle class="background" :cx="center" :cy="center" :r="radius"></circle>  
    <circle class="progress" id="progress" :cx="center" :cy="center" :r="radius" ref="progress"></circle>
    <text x="50%" y="50%" text-anchor="middle" dy=".3em">
      <tspan>{{currentPercentage}}</tspan>
      <tspan class="text-xs">%</tspan>
    </text>
  </svg>
</template>

<script>
export default {
  name: 'Gauge',
  props: {
    currentValue: {required: true},
    maxValue: {required: false, default: 100},
    animated: {required: false, default: true}
  },
  computed: {
    radius () {
      return this.maxValue / (2 * Math.PI)
    },
    center () {
      return this.radius + 2
    }, 
    box () {
      return this.center * 2
    },
    endPercentage () {
      return 100 * parseFloat(this.currentValue) / parseFloat(this.maxValue)
    },
    dashOffet () {
      return parseFloat(getComputedStyle(this.$refs.progress)['stroke-dashoffset'])
    },
    currentPercentage () {
      return Math.floor((100 * (this.maxValue - this.dashOffset)) / this.maxValue) || 0
    }
  },
  methods: {
    updatePercentage () {
      if (this.currentPercentage < this.endPercentage) {
        window.requestAnimationFrame(this.updatePercentage)
      }
    },
    animateProgress (percent) {
      var offset = this.maxValue - (this.maxValue * percent / 100)
      this.$refs.progress.setAttribute('stroke-dashoffset', offset)
      window.requestAnimationFrame(this.updatePercentage)
    }
  },
  mounted () {
    this.$refs.progress.setAttribute('stroke-dasharray', this.maxValue)
    this.$refs.progress.setAttribute('stroke-dashoffset', this.maxValue)

    setTimeout(() => {
      this.animateProgress(this.endPercentage)
    }, 0)
  }
}
</script>

<style scoped>
  .progress {
    stroke: currentColor;
    stroke-width: 4;
    transition: all 4s ease-in;
    transform: rotate(-90deg);
    transform-origin: 50%;
  }
  circle {
    fill: none;
    stroke-width: 4;
  }
  .background {
    stroke: #dcdcdc;
  }
  text {
    font-family: system-ui, sans-serif;
    font-weight: 100;
    font-size: 0.75rem;
    fill: currentColor;
  }
  .text-xs {
    font-size: 0.45rem;
  }
</style>
