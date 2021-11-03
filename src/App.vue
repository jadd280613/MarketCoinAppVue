<template>
    <div class="container">
        <div class="row">
            <h1 style="text-align:center">CoinMarket App in Vue JS</h1>
            <input
                type="text"
                class="form-control text-light bg-dark rounded-0 border-0 my-4"
                placeholder="Search"
                v-model="textSearch"
                @keyup="searchCoin()"
                autofocus
            />
            <Table :coins="filteredCoins" />
        </div>
    </div>
</template>

<script>
import Table from '../src/components/Table.vue'

export default {
    name: 'App',
    components: {
      Table
    },
    data() {
        return {
            coins: [],
            filteredCoins: [],
            titles: ['#', 'Coin', 'Price', 'Price Change', '24h Volume'],
            textSearch: '',
        };
    },
    async mounted() {
        const res = await fetch(
            'https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false'
        );
        const data = await res.json();
        this.coins = data;
        this.filteredCoins = data;
    },
    methods: {
        searchCoin() {
            this.filteredCoins = this.coins.filter(
                (coin) =>
                    coin.name
                        .toLowerCase()
                        .includes(this.textSearch.toLowerCase()) ||
                    coin.symbol
                        .toLowerCase()
                        .includes(this.textSearch.toLowerCase())
            );
        },
    },
};
</script>
