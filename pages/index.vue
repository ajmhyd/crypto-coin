<template>
  <div>
    <TheHeroText />
    <div class="container mx-auto flex justify-center mt-8">
      <button
        v-if="$fetchState.pending"
        type="button"
        class="animate-pulse inline-flex items-center px-6 py-3 border border-transparent text-base font-medium rounded-md shadow-sm text-white bg-green-600 hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500"
        disabled
      >
        Loading...
      </button>
      <div
        v-else-if="$fetchState.error"
        class="rounded-md bg-red-50 p-4 max-w-lg"
      >
        <div class="flex">
          <div class="flex-shrink-0">
            <svg
              class="h-5 w-5 text-red-400"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 20 20"
              fill="currentColor"
              aria-hidden="true"
            >
              <path
                fill-rule="evenodd"
                d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z"
                clip-rule="evenodd"
              />
            </svg>
          </div>
          <div class="ml-3">
            <h3 class="text-sm font-medium text-red-800">
              Error fetching data. Please reload the page.
            </h3>
          </div>
        </div>
      </div>
      <div v-else>
        <div v-if="coin">
          <div class="mx-auto flex justify-center">
            <button
              type="button"
              class="inline-flex items-center px-6 py-3 border border-transparent text-base font-medium rounded-md shadow-sm text-white bg-green-600 hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500"
              @click="
                setCoin(null)
                setSearch('')
              "
            >
              <svg
                class="-ml-1 mr-2 h-5 w-5"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M12.066 11.2a1 1 0 000 1.6l5.334 4A1 1 0 0019 16V8a1 1 0 00-1.6-.8l-5.333 4zM4.066 11.2a1 1 0 000 1.6l5.334 4A1 1 0 0011 16V8a1 1 0 00-1.6-.8l-5.334 4z"
                />
              </svg>
              Back
            </button>
          </div>
          <CoinDetail :coin="coin" @updateCoin="setCoint($event)" />
        </div>
        <div v-else>
          <div class="mx-auto flex justify-center">
            <button
              type="button"
              class="inline-flex items-center px-6 py-3 border border-transparent text-base font-medium rounded-md shadow-sm text-white bg-green-600 hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500"
              @click="$fetch"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
                fill="currentColor"
                class="-ml-1 mr-2 h-5 w-5"
              >
                <path
                  fill-rule="evenodd"
                  d="M4 2a1 1 0 011 1v2.101a7.002 7.002 0 0111.601 2.566 1 1 0 11-1.885.666A5.002 5.002 0 005.999 7H9a1 1 0 010 2H4a1 1 0 01-1-1V3a1 1 0 011-1zm.008 9.057a1 1 0 011.276.61A5.002 5.002 0 0014.001 13H11a1 1 0 110-2h5a1 1 0 011 1v5a1 1 0 11-2 0v-2.101a7.002 7.002 0 01-11.601-2.566 1 1 0 01.61-1.276z"
                  clip-rule="evenodd"
                />
              </svg>
              Refresh
            </button>
          </div>
          <TheSearchBar @update="setSearch($event)" />
          <CoinList
            :search="search"
            :coin-list="searchedList"
            @updateCoin="setCoin($event)"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Fuse from 'fuse.js'

const options = {
  threshold: 0.4,
  distance: 10,
  keys: ['name', 'symbol'],
}

export default {
  data() {
    return {
      search: '',
      coin: null,
      searchedList: [],
      fuse: null,
    }
  },
  async fetch() {
    try {
      const coins = await this.$axios.$get('/tickers')
      this.fuse = new Fuse(coins, options)
      this.coinList = coins
      this.searchedList = coins
    } catch (error) {
      error({
        statusCode: 503,
        message: 'Unable to fetch',
      })
    }
  },
  watch: {
    search(value) {
      if (value === '') {
        this.searchedList = this.coinList
      } else {
        // fuse.search() returns array of "items". Mapping over array to remove "items" to keep consistancy with shape of list
        this.searchedList = this.fuse.search(value).map((item) => item.item)
      }
    },
  },
  methods: {
    setSearch(value) {
      this.search = value
    },
    setCoin(value) {
      this.coin = value
    },
  },
}
</script>
