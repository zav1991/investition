<template>
  <div id="app">
    <Investment />
  </div>
</template>

<script>
import Investment from "./components/Investment";

export default {
  name: 'app',
  components: {
    Investment,
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
    display: flex;
    justify-content: center;
  }

  body {
    background-color: bisque;
    height: 100%;
    width: 100%;
  }
</style>
