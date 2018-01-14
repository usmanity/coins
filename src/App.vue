<template>
  <div id="app">
    <ul>
      <li v-for="coin in coinData" :class="cursor(coin)">
        <div class="info">
          <span class="rank">{{ coin.rank }}</span> 
          <span class="name">{{ coin.name }}</span>
        </div>
        <div class="pricing">
          <span class="price">
            ${{ coin.price_usd }}
          </span>
          <span class="change">
            {{ coin.percent_change_24h }}
          </span>
        </div>
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
      coinData: {},
    }
  },
  methods: {
    getCoinData() {
      var self = this;
      axios.get('https://api.coinmarketcap.com/v1/ticker/?limit=10').then(function(res){
        self.coinData = res.data;
      });
    },
    cursor(coin) {
      if (!coin) {
        return 'ew-resize'
      }
      if (coin.percent_change_24h < 0) {
        return 's-resize'
      } else {
        return 'n-resize'
      }
    }
  },
  mounted() {
    this.getCoinData();
  }
}
</script>

<style lang="scss">
body {
  width: 300px;
  height: 420px;
  // border: 1px solid;
  font-size: 16px;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  overflow-y: auto;
  width: 100%;
  height: 100%;
}
h1 {
  text-align: center;
}
ul {
  padding: 0;
  margin: 0;
}
li {
  list-style: none;
  padding: 4px;
  border-radius: 1px;
  display: flex;
  &:hover {
    background-color: rgba(120, 120, 120, 0.3);
  }
}
.info {
  width: 50%;
  display: flex;
  align-items: center;
  .rank {
    padding-right: 10px;
    font-size: 10px;
  }
}

.pricing {
  display: flex;
  flex-direction: column;
  width: 50%;
  text-align: right;
  font-family: monaco, monospace;
  .change {
    font-size: 12px;
  }
  .price {
    font-size: 13px;
  }
}

.n-resize        { cursor: n-resize; }
.s-resize        { cursor: s-resize; }
.ew-resize       { cursor: ew-resize; }
</style>
