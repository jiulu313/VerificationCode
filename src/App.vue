<template>
  <div id="app">
    <sliding-block></sliding-block>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import SlidingBlock from './components/SlidingBlock'

export default {
  name: 'App',
  components: {
    HelloWorld,
    SlidingBlock
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
