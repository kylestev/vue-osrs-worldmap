<template>
  <l-control position="topleft">
    <v-select
      :options="locations"
      v-model="kappa"
    >
      <template slot="option" slot-scope="option">
        {{ option.name }}
      </template>
    </v-select>
  </l-control>
</template>

<script>
import { LControl } from 'vue2-leaflet'
import VSelect from 'vue-select'

export default {
  props: {
    locations: {
      type: Array,
      required: true
    }
  },

  data () {
    return {
      kappa: null
    }
  },

  components: {
    LControl,
    VSelect
  }
}
</script>
