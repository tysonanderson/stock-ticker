<template>
  <div class="stock mx-auto">
        <b-row>
          <b-col lg="3">
            <p class="symbol">{{ quote.symbol }}</p>
            <p class="name">{{ quote.companyName }}</p>
          </b-col>
          <b-col lg="5">
            <StockLine :symbol="quote.symbol" :color="changeColor"/>
          </b-col>
        <b-col lg="2">
          <p class="column-label">LAST</p>
          <p class="price">{{ quote.latestPrice }}</p>
        </b-col>
        <b-col lg="2">
          <p class="column-label">CHANGE</p>
          <p class="change" :style="{ color: changeColor }">{{ quote.changePercent | perc }}</p>
        </b-col>
        </b-row>
      
  </div>
</template>

<script>
import * as d3 from "d3";
import StockLine from './StockLine.vue'

export default {
  name: 'Stock',
  components: {
    StockLine
  },
  props: {
    quote: Object,
    aid: String
  },
  computed: {
    changeColor: function () {
      return (this.quote.changePercent < 0) ? "red" : "green"
    }
  },
  filters:{
    perc: value => {
      return d3.format(".1%")(value)
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .column-label{
    color: #999;
    margin-top: .2em;
  }
  .stock {
    border-bottom: 1px solid #ccc;
    border-left: 1px solid #ccc;
    border-right: 1px solid #ccc;
    padding-left: .7em;
    font-family: Roboto;
    background: white;
  }
  .symbol {
    font-weight: 700;
    font-size: 2em;
    margin-bottom: -.1em;
  }
  .name {
    font-weight: 100;
    color: #999;
    margin-bottom: .1em;
  }
  .price {
    font-size: 1.2em;
    font-weight: 400;
/*    margin-top: .2em;*/
  }
  .change {
    font-size: 1.2em;
    font-weight: 100;
/*    margin-top: .2em;*/
  }
</style>
