<template>
  <v-sheet
    width="100%"
    height="100%"
  >
    <spinning-logo
      v-if="!iframe_loaded"
      size="15%"
      subtitle="Loading external application..."
    />
    <iframe
      v-show="iframe_loaded"
      :src="source"
      height="100%"
      width="100%"
      @load="loadFinished"
    />
  </v-sheet>
</template>

<script lang="ts">
import Vue from 'vue'

import SpinningLogo from '../common/SpinningLogo.vue'

export default Vue.extend({
  name: 'Iframe',
  components: {
    SpinningLogo,
  },
  props: {
    source: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      iframe_loaded: false,
    }
  },
  methods: {
    loadFinished(): void {
      this.iframe_loaded = true
    },
  },
})
</script>
