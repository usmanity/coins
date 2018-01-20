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
    <div class="footer">
      <div class="change-range">
        Change is over the past 24 hours
      </div>
    </div>
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
      query: '',
      ls: this.$parent.$ls
    }
  },
  watch: {
    query(value) {
      this.search(value);
    }
  },
  methods: {
    getCoinData() {
      var now = new Date().getTime() / 1000;
      var self = this;
      var cachedResults = self.ls.get('coinData');
      var cacheTimestamp = self.ls.get('cacheTimestamp');
      if (cachedResults &&  cacheTimestamp > now - 60) {
        self.coinData = cachedResults;
        self.currentCoins = cachedResults.slice(0, 10);
        return;
      }
      axios.get('https://api.coinmarketcap.com/v1/ticker/?limit=100').then(function(res){
        var now = new Date().getTime() / 1000;
        self.coinData = res.data;
        self.currentCoins = self.coinData.slice(0, 10);
        self.ls.set('coinData', res.data);
        self.ls.set('cacheTimestamp', now);
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
      if (value === '') {
        this.currentCoins = this.coinData.slice(0, 10);
        return;
      }
      this.currentCoins = _.filter(this.coinData, (coin) => {
        if (coin.name.toLowerCase().indexOf(value) !== -1 || coin.symbol.toLowerCase().indexOf(value) !== -1) {
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
  height: 470px;
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
  padding: 3px 4px;
  border-radius: 1px;
  display: flex;
  border-bottom: 1px solid rgba(124, 124, 124, 0.2);
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

.n-resize { 
  cursor: n-resize;
  .change {
    color: green;
  }
}
.s-resize { 
  cursor: s-resize;
  .change {
    color: red;
  }
}
.ew-resize { 
  cursor: ew-resize;
}
#search {
  width: 100%;
  font-size: 18px;
  border: 1px solid #ccc;
  margin: 4px 0;
  height: 40px;
  box-sizing: border-box;
  padding: 5px 10px;
  font-weight: bold;
  border-radius: 1px;
  transition: border 200ms ease-out;
  &:focus {
    outline: none;
    border-color: rgba(100, 100, 100, 0.9);
    transition: border 200ms ease-in;
  }
}
.change-range {
  font-size: 8px;
  text-align: center;
  padding-top: 4px;
}
</style>
