<template>
  <div id="app">
    <mainPage />
  </div>
</template>

<script>
import mainPage from "./components/mainPage";

import('./assets/styles/fontAdder.css')
import('./assets/styles/main.scss')
import('./assets/styles/hexagonStyle.scss')
import('./assets/styles/topMenuStyle.scss')


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
