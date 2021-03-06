<template>
  <l-map
    ref="map"
    :max-zoom="11"
    @update:zoom="x => { zoom = x }"
    :options="{ zoomControl: false }"
  >
    <rs-tile-layer :floor="floor"/>
    <!-- We need to wait for the $nextTick callback to fire -->
    <template v-if="map && showLabels">
      <!-- Render all the locations -->
      <rs-location-marker
        v-for="(location, $x) in locations"
        :key="$x"
        :label="location.name"
        :location="location.coords"
        :map="map"
        :zoom="zoom"
      />
    </template>
  </l-map>
</template>

<script>
import { LMap } from 'vue2-leaflet'
import RsTileLayer from './RsTileLayer'
import RsLocationMarker from './RsLocationMarker'

import { Position } from '../model/Position'
import locations from '../data/locations.json'

export default {
  props: {
    showLabels: {
      type: Boolean,
      required: false,
      default: false
    },
    floor: {
      type: Number,
      required: false,
      default: 0
    },
    tile: {
      type: Object,
      required: false,
      default: function () {
        return new Position(3101, 3094, 0)
      }
    }
  },

  components: {
    LMap,
    RsLocationMarker,
    RsTileLayer
  },

  data () {
    return {
      color: 'cyan',
      map: null,
      locations,
      zoom: 7,
      activeTileRect: null
    }
  },

  computed: {
  },

  methods: {
    redraw () {
      const map = this.map
      const tile = this.tile
      if (this.activeTileRect !== null) {
        map.removeLayer(this.activeTileRect)
      }

      this.activeTileRect = tile.toLeaflet(map)
      this.activeTileRect.addTo(map)

      map.setView(tile.toLatLng(map), this.zoom)
    }
  },

  mounted () {
    this.$nextTick(() => {
      window.map = this.map = this.$refs.map.mapObject
      if (this.tile) {
        this.redraw()
      } else {
        this.map.setView([-82, -148], this.zoom)
      }
    })
  },

  watch: {
    tile (x) {
      if (this.map && x) {
        this.redraw()
      }
    }
  }
}
</script>
