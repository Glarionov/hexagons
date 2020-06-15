<template>
  <div id="app">
    <mainPage />
  </div>
</template>

<script>
import mainPage from "./components/mainPage";

import('./assets/styles/fontAdder.css')
import('./assets/styles/main.css')
import('./assets/styles/hexagonStyle.css')
import('./assets/styles/topMenuStyle.css')


export default {
  name: 'App',
  components: {
    mainPage
  }
}
</script>

<style>
#app {
  font-family: 'pfdintextcondpro-medium', Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
   height: 100%;
  font-stretch: condensed;
}
</style>
