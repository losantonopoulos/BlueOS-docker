<template>
  <v-card
    width="500"
    elevation="0"
    class="mx-auto my-6 injectors-list"
  >
    <v-list
      v-if="are_nmea_sockets_available && !updating_nmea_sockets"
      style="background-color: transparent"
      dense
    >
      <template v-for="(nmea_socket, index) in available_nmea_sockets">
        <v-divider
          v-if="index!==0"
          :key="index"
        />
        <v-list-item
          :key="index"
          class="pa-0"
        >
          <nmea-socket-card :nmea-socket="nmea_socket" />
        </v-list-item>
      </template>
    </v-list>

    <v-container v-else-if="updating_nmea_sockets">
      <spinning-logo
        size="30%"
        subtitle="Fetching available NMEA sockets..."
      />
    </v-container>
    <v-container
      v-else
      class="text-center"
    >
      <p class="text-h6">
        No NMEA sockets available.
      </p>
    </v-container>

    <v-fab-transition>
      <v-btn
        :key="'create_button'"
        color="primary"
        fab
        large
        dark
        fixed
        bottom
        right
        class="v-btn--example"
        @click="openCreationDialog"
      >
        <v-icon>mdi-plus</v-icon>
      </v-btn>
    </v-fab-transition>

    <creation-dialog
      v-model="show_creation_dialog"
    />
  </v-card>
</template>

<script lang="ts">
import Vue from 'vue'

import nmea_injector from '@/store/nmea-injector'
import { NMEASocket } from '@/types/nmea-injector'

import SpinningLogo from '../common/SpinningLogo.vue'
import NMEASocketCard from './NMEASocketCard.vue'
import CreationDialog from './NMEASocketCreationDialog.vue'

export default Vue.extend({
  name: 'NMEAInjector',
  components: {
    SpinningLogo,
    CreationDialog,
    'nmea-socket-card': NMEASocketCard,
  },
  data() {
    return {
      show_creation_dialog: false,
    }
  },
  computed: {
    updating_nmea_sockets(): boolean {
      return nmea_injector.updating_nmea_sockets
    },
    available_nmea_sockets(): NMEASocket[] {
      return nmea_injector.available_nmea_sockets
    },
    are_nmea_sockets_available(): boolean {
      return !this.available_nmea_sockets.isEmpty()
    },
  },
  methods: {
    openCreationDialog(): void {
      this.show_creation_dialog = true
    },
  },
})
</script>

<style scoped>
.injectors-list {
  background-color: transparent;
  max-width: 70%;
  margin: auto;
}
</style>
