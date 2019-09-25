<template>
    <v-layout column>
        <p class="text-center" style="font-size: 48px">{{ticker}}</p>
        <GChart
            v-if="webSocketOpen"
            type="LineChart"
            :data="priceHistory"
            :options="chartOptions"
        />
        <p class="text-center" v-if="!webSocketOpen">Something went wrong with the socket</p>
        <v-card-text align="center" :class="color">{{price}}</v-card-text>
    </v-layout>
</template>


<script>
import {GChart} from 'vue-google-charts'

export default {
  props: {
      ticker: String
  },
  components: {
      GChart
  },
  data: () => ({
      price: 0,
      previousPrice: 0,
      priceHistory: [
          ['Time', 'Stock Price']
      ],
      chartOptions: {
          chart: {
              title: "Stock Price",
          }
      },
      color: 'white',
      webSocketOpen: false
  }),
  mounted() {
    let ws = new WebSocket(`ws://localhost:9000/socket/${this.ticker}`)

    ws.addEventListener('open', (e) => {
        this.webSocketOpen = true
    })

    ws.addEventListener('message', (e) => {
        let updateEvent = JSON.parse(e.data)

        this.previousPrice = this.price
        this.price = updateEvent.price
        this.priceHistory.push([new Date().toLocaleString(), updateEvent.price])

        if(this.price > this.previousPrice) {
            this.color = "green"
        }

        if(this.price < this.previousPrice) {
            this.color = "red"
        }

        if(this.price === this.previousPrice) {
            this.color = "grey"
        }
    })

    ws.addEventListener('close', (e) => {
        this.color = "yellow"
        this.webSocketOpen = false
    })
  }
};
</script>
