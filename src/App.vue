<template>
<div class="body">
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App" />
  </div>
</div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.body::before {
  content: "";
  position: fixed;
  top: -10px;
  left: 0;
  width: 100%;
  height: 10px;
  box-shadow: 0px 0 10px rgba(0, 0, 0, 0.8);
  z-index: 100;
}

.body {
  background: radial-gradient(ellipse at center, rgba(194, 234, 219, 1) 0%, rgba(227, 215, 242, 1) 74%, rgba(227, 215, 242, 1) 74%);
  background-size: 300%;
  padding: 3em;
  color: #333;
  font-family: 'Source Sans Pro';
  font-size: 20px;
  font-weight: 300;
}
</style>
