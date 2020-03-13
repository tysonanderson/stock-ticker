<template>
  <b-container id="app" fluid>
    <b-row class="justify-content-center">
      <b-col col lg="8" md="10" sm="12">
        <Stock :quote="stock.quote" v-for="stock in stocks" :key="stock.quote.symbol"/>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import Stock from './components/Stock.vue'
// import PeriodPicker from './components/PeriodPicker.vue'

export default {
  name: 'App',
  components: {
    Stock,
    // PeriodPicker
  },
  data () {
    return {
      stocks: [],
      period: "1m",
      symbols: ["aapl","fb","tsla","alk","twtr","ADBE", "DAL", "JBLU", "amzn"],
      text: "test text"
    }
  },
  mounted () {
    const stockPath = `https://cloud.iexapis.com/stable/stock/market/batch?symbols=${this.symbols.join(',')}&types=quote&range=${this.period}&last=5&token=pk_5eb47f2284984b7e9c1519e0429d7ff7`;
    this.$http.get(stockPath)
    .then( result => this.stocks = result.data)

  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto:100,400,700&display=swap');

.stock:last-child{
  border-radius: 0px 0px 6px 6px;
}
.stock:first-child{
  border-radius: 6px 6px 0px 0px ;
}
#app{
  margin-top: 2em;
}
body { 
  background: #2c3e50 !important
}; 

</style>
