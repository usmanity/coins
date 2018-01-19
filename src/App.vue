<template>
  <div id="app">
    <div class="add-coins">
      <input type="search" v-model="query" name="search" id="search" placeholder="Search">
    </div>
    <ul>
      <li v-for="coin in currentCoins" :class="cursor(coin)">
        <div class="info">
          <span class="rank">{{ coin.rank }}</span> 
          <span class="name">{{ coin.name }}</span>
        </div>
        <div class="pricing">
          <span class="price">
            ${{ coin.price_usd }}
          </span>
          <span class="change">
            {{ coin.percent_change_24h }}%
          </span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';
import _ from 'underscore';

export default {
  name: 'app',
  data () {
    return {
      coinData: {},
      currentCoins: {},
      query: ''
    }
  },
  watch: {
    query(value) {
      this.search(value);
    }
  },
  methods: {
    getCoinData() {
      var self = this;
      axios.get('https://api.coinmarketcap.com/v1/ticker/?limit=10').then(function(res){
        console.log(res);
        self.coinData = res.data;
        self.currentCoins = res.data;
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
    },
    search(value){
      console.log(value);
      if (value === '') {
        this.currentCoins = this.coinData;
        return;
      }
      this.currentCoins = _.filter(this.coinData, (coin) => {
        if (coin.name.toLowerCase().indexOf(value) !== -1) {
          return coin;
        }
      });

    },
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
    margin-left: -2px;
    text-align: center;
    font-size: 10px;
    width: 20px;
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
#search {
  width: 100%;
  font-size: 18px;
  border: 1px solid #ccc;
}
</style>
