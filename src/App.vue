<template>
  <div id="app">
    <Hexaland></Hexaland>

  </div>
</template>

<script>
import Hexaland from "./components/Hexaland";
import('./assets/styles/main.css')

export default {
  name: 'App',
  components: {
    Hexaland,
  }
}
</script>

<style>
#app {
  font-family: 'pfdintextpro-medium', Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000312;
  background: red;
          height: 100%;
  /*margin-top: 60px;*/
}
</style>
