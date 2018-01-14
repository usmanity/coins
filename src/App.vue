<template>
  <div id="app">
    <h1>{{ msg }}</h1>
    <ul>
      <li v-for="coin in coinData">
        {{ coin.name }} (${{ coin.price_usd }})
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'app',
  data () {
    return {
      msg: 'Coins',
      coinData: {},
    }
  },
  methods: {
    getCoinData() {
      var self = this;
      axios.get('https://api.coinmarketcap.com/v1/ticker/?limit=10').then(function(res){
        self.coinData = res.data;
      });
    }
  },
  mounted() {
    this.getCoinData();
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
h1 {
  text-align: center;
}
body {
  width: 300px;
  height: 400px;
  border: 1px solid;
}
</style>
