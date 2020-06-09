<template>
  <div id="app">
    <Hexaland></Hexaland>

  </div>
</template>

<script>
import Hexaland from "./components/Hexaland";

export default {
  name: 'App',
  components: {
    Hexaland,
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
  background: red;
          height: 100%;
  /*margin-top: 60px;*/
}
</style>
