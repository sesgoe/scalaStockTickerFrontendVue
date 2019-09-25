<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-layout align-center justify-center>
        <v-card>
          <v-card-title>Add New Ticker</v-card-title>
          <v-card-actions>
            <v-text-field v-model="newTicker"></v-text-field>
          </v-card-actions>
          <v-card-actions>
            <v-layout align-center justify-center>
              <v-btn @click="addTicker" color="success">Add</v-btn>
            </v-layout>
          </v-card-actions>
        </v-card>
      </v-layout>
    </v-row>
    <v-row no-gutters>
      <v-col
        v-for="n in stockList"
        :key="n"
        cols="12"
        sm="4"
      >
        <v-card
        outlined
        class="ma-4"
        >
          <v-layout align-end>
            <v-btn x-small color="red" @click="removeTicker(n)">X</v-btn>
          </v-layout>
          <StockTicker :ticker="n"/>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import StockTicker from './StockTicker';

export default {
  data: () => ({
    stockList : ["AMZN", "TSLA", "GOOG"],
    newTicker: ""
  }),
  components: {
    StockTicker
  },
  methods: {
    addTicker: function() {
      if(this.stockList.includes(this.newTicker)) {
        alert('That ticker is already being displayed!')
        this.newTicker = ""
        return
      }
      this.stockList.push(this.newTicker)
      this.newTicker = ""
    },
    removeTicker: function(tickerName) {
      this.stockList.splice(this.stockList.indexOf(tickerName), 1)
    }
  }
};
</script>
