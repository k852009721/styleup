<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Calendar msg="Calendar"/>
    <Uploads msg="Uploads"/>

  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Calendar from './components/Calendar.vue'
import Uploads from './components/Uploads.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld,
    Calendar,
    Uploads,
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
