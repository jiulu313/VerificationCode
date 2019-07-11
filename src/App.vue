<template>
  <div id="app">
    <div class="root_container">
      <sliding-block style="margin-left: 40px;"></sliding-block>

    </div>
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

.root_container {
  width: 600px;
  height: 600px;
  background-color: cadetblue;
}
</style>
