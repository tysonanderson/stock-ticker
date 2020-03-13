<template>
  <div class="stock-line" ref="sl" d-none>
    <svg :width="width" :height="height">
      <circle :fill="color" :cx="c.x" :cy="c.y" r=2.5></circle>
      <path :d="d" :style="{stroke: this.color}" class="sparkline"></path>
    </svg>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  name: 'StockLine',
  props: {
    symbol: String,
    color: String
  },
  data () {
    return {
      d: "",
      dateExtents: [],
      priceExtents: [],
      prices: [],
      width: 0,
      height: 70,
      c: {},
    }
  },
  mounted (){
    const stockPath = `https://cloud.iexapis.com/stable/stock/${this.symbol}/intraday-prices?token=pk_5eb47f2284984b7e9c1519e0429d7ff7`
    this.$http.get(stockPath)
    .then(
      result => {
        this.prices = result.data.filter( d => d.close)
        result.data.map(d => d.close)
        this.prices.pDate = this.prices.forEach(d => d.pDate = Date.parse(`${d.date}T${d.minute}`))
        this.priceExtents = d3.extent(result.data, d => d.close)
        this.dateExtents = d3.extent(result.data, d => d.pDate)

        window.addEventListener('resize', this.drawLine)
        
        this.drawLine();
        
      }
    )
  },
  methods: {
    drawLine: function() {
      this.width = this.$refs.sl.clientWidth;
      const y = d3.scaleLinear().domain(this.priceExtents).range([this.height - 10,10]);
      const x = d3.scaleLinear().domain(this.dateExtents).range([0,this.width - 10]);

      const line = d3.line()
        .x(d => x(d.pDate))
        .y(d => y(d.close))

      const sparkline = line(this.prices)
      this.d = sparkline

      const latest = this.prices[this.prices.length - 1]
      this.c = {x: x(latest.pDate), y: y(latest.close)}
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.stock-line{
  margin-top: 1em;
}
.sparkline{
  fill: none;
}
</style>
