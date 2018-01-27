<template>
  <div class="top-ten">
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
export default {
  name: 'topTen',
  data() {
      return {
        coinData: {},
        currentCoins: {},
      };
  },
  methods: {
      getCoinData() {
      var now = new Date().getTime() / 1000;
      var self = this;
      var cachedResults = self.ls.get('coinData');
      var cacheTimestamp = self.ls.get('cacheTimestamp');
      if (cachedResults &&  cacheTimestamp > now - 600) {
        console.log('Getting data from cache');
        self.coinData = cachedResults;
        self.currentCoins = cachedResults.slice(0, 10);
        return;
      }
      axios.get('https://api.coinmarketcap.com/v1/ticker/?limit=100').then(function(res){
        var now = new Date().getTime() / 1000;
        self.coinData = res.data;
        self.currentCoins = self.coinData.slice(0, 10);

        // crude caching in localStorage.
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
      return 'ew-resize';
    },
    mounted() {
        this.getCoinData();
    },
  },
}
</script>
<style lang="scss" scoped>
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
</style>
