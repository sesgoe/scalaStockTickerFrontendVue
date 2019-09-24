<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Clutch </span>
        <span class="font-weight-light">Stock Ticker UI</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <span>Ses Goe</span>
    </v-app-bar>

    <v-content>
      <StockDisplayWrapper/>
    </v-content>
  </v-app>
</template>

<script>
import StockDisplayWrapper from './components/StockDisplayWrapper';

export default {
  name: 'App',
  components: {
    StockDisplayWrapper,
  },
  data: () => ({
    //
  }),
};
</script>
