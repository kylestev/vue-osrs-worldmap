<template>
  <div id="app" style="height: 100%">
    <rs-map ref="rsMap" :floor="floor" :tile="tile" @mapready="x => { map = x }"/>
  </div>
</template>

<script>
import { Position } from './model/Position'

export default {
  data () {
    return {
      tile: null,
      map: null,
      floor: 0
    }
  },
  mounted () {
    const [_, x, y, z] = /#tile=(\d+),(\d+)(?:,(\d+))?/.exec(window.location.hash).map(x => parseInt(x))

    console.log({ x, y, z })
    if (x && y) {
      this.tile = new Position(x, y, z)
      this.floor = z || 0
    }
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
