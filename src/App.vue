<template>
  <div id="app">
    <TreeChart />
  </div>
</template>

<script>
import TreeChart from './components/TreeChart.vue'

export default {
  name: 'App',
  components: {
    TreeChart
  }
}
</script>

<style>
body {
  width: 100vw;
  height: 100vh; 
  overflow: hidden;
  font-family: 'Roboto', sans-serif;
}
</style>
