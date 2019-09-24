<template>
    <v-card
        outlined
        class="ma-4"
    >
        <p class="text-center" style="font-size: 48px">{{ticker}}</p>
        <v-card-text align="center" :class="color">{{price}}</v-card-text>
    </v-card>
</template>


<script>
export default {
  props: {
      ticker: String
  },
  data: () => ({
      price: 0,
      previousPrice: 0,
      color: 'white'
  }),
  mounted() {
    let ws = new WebSocket("ws://localhost:9000/socket")

    ws.addEventListener('open', (e) => {
        ws.send(`{"symbol": "${this.ticker}"}`)
    });

    ws.addEventListener('message', (e) => {
        let updateEvent = JSON.parse(e.data)

        this.previousPrice = this.price
        this.price = updateEvent.price

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
    })
  }
};
</script>
