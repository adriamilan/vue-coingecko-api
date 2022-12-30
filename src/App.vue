<template>
  <div class="container my-5">

    <h1>CoinMarket with VueJS💚</h1>

    <input type="text" class="form-control bg-light rounded border my-3" placeholder="Bitcoin, Ethereum, Dogecoin, Cardano..." @keyup="searchCoin()" v-model="textSearch">

    <table class="table table-striped">
      <thead>
        <tr>
          <th v-for="title in titles" :key="title">
            {{ title }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
          <td>
            {{ index + 1 }}
          </td>
          <td>
            <img :src="coin.image" style="width: 2rem;" class="me-2">
            <span>{{ coin.name }}</span> <span class="text-muted text-uppercase">{{ coin.symbol }}</span>
          </td>
          <td>
            {{ coin.current_price }} €
          </td>
          <td :class="[coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger']">
            {{ coin.price_change_percentage_24h }} %
          </td>
          <td>
            {{ coin.total_volume }} €
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="text-center">
    <p>API powered by <a href="https://www.coingecko.com/">coingecko.com</a> <br> Thxs to <a href="https://www.youtube.com/c/FaztCode">FaztCode</a>💚</p>
  </div>

</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: ['#', 'Coins', 'Price', 'Prince Change', '24h Volume'],
      textSearch: ''
    }
  },
  async mounted() {
    const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=eur&order=market_cap_desc&per_page=100&page=1&sparkline=false')
    const data = await res.json()
    this.coins = data
    this.filteredCoins = data
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter(coin => coin.name.toLowerCase().includes(this.textSearch.toLocaleLowerCase()) || coin.symbol.toLowerCase().includes(this.textSearch.toLocaleLowerCase())) 
    }
  }
}
</script>

<style>

</style>
