<template>
  <div id="app">
    <DragDropDemo />
  </div>
</template>

<script>
import DragDropDemo from './components/DragDropDemo.vue'

export default {
  name: 'App',
  components: {
    DragDropDemo
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 0;
  padding: 0;
  height: 100vh;
}
</style>
