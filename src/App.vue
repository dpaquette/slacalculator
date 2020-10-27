<template>
  <v-app>


    <v-main>
      <DowntimeCalculator/>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';
import DowntimeCalculator from './components/DowntimeCalculator.vue';

export default Vue.extend({
  name: 'App',

  components: {
    DowntimeCalculator,
  },

  data: () => ({
    //
  }),
});
</script>
