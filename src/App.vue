<template>
  <rs-map id="app"/>
</template>

<script>
import RsMap from './components/RsMap'

export default {
  components: {
    RsMap
  }
}
</script>

<style lang="scss">
html, body {
  margin: 0;
  height: 100vh;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
