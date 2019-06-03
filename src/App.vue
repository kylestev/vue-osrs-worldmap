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
  methods: {
    parseLocation () {
      const params = new URLSearchParams(location.hash.replace(/^#/, ''))
      const tile = params.get('tile') || '3223,3220,1'
      const [x, y, z] = tile.split(',').map(x => parseInt(x))

      const zoom = params.get('zoom') || '9'
      if (zoom) {
        this.$refs.rsMap.zoom = parseInt(zoom)
      }

      if (x && y) {
        return new Position(x, y, z || 0)
      }
    },
    updateLocation () {
      const location = this.parseLocation()
      if (location) {
        this.tile = location
        this.floor = location.z
      } else {
        this.tile = null
        this.floor = 0
      }
    }
  },
  mounted () {
    window.onhashchange = () => {
      this.updateLocation()
    }

    this.updateLocation()
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
