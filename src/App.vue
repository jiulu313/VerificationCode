<template>
  <div id="app">
    <div class="root_container">
      <sliding-block2 style="margin-left: 40px;"></sliding-block2>

    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import SlidingBlock2 from './components/SlidingBlock2'

export default {
  name: 'App',
  components: {
    HelloWorld,
    SlidingBlock2
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

.root_container {
  width: 600px;
  height: 600px;
  background-color: cadetblue;
}
</style>
