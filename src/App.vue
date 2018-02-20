<template>
  <div id="app">
    <div class="add-coins">
      <input type="search" v-model="query" name="search" id="search" placeholder="Search">
    </div>
    <top-ten :query="query"></top-ten>
    
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
import topTen from './TopTen';

export default {
  name: 'app',
  components: {
    topTen,
  },
  data () {
    return {
      ls: this.$parent.$ls,
      currentCoins: '',
      query: '',
    }
  },
  watch: {
  },
  methods: {
  },
  mounted() {
  }
}
</script>

<style lang="scss">
body {
  width: 300px;
  height: 470px;
  // border: 1px solid;
  font-size: 16px;
  -webkit-font-smoothing: antialiased;
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
