<template>
  <div id="app">
    <div class="orange">
      <gauge currentValue="83" maxValue="100"></gauge>
    </div>
  </div>
</template>

<script>
import Gauge from './components/Gauge.vue'

export default {
  name: 'app',
  components: {
    Gauge
  }
}
</script>

<style>
  .orange {
    color: #FB4514;
  }
</style>
