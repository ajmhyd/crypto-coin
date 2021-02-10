<template>
  <div class="contianer mx-auto flex justify-center mt-8">
    <ul v-if="coinList.length">
      <div class="h-96">
        <RecycleScroller
          v-slot="{ item }"
          class="scroller"
          :items="searchedList"
          :item-size="64"
          key-field="id"
        >
          <div class="divide-y">
            <CoinListItem :coin="item" />
          </div>
        </RecycleScroller>
      </div>
    </ul>
    <button
      v-if="loading"
      type="button"
      class="animate-pulse inline-flex items-center px-6 py-3 border border-transparent text-base font-medium rounded-md shadow-sm text-white bg-green-600 hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500"
      disabled
    >
      Loading...
    </button>
    <div v-if="error" class="rounded-md bg-red-50 p-4">
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
  </div>
</template>

<script>
import Fuse from 'fuse.js'
import { RecycleScroller } from 'vue-virtual-scroller'
import 'vue-virtual-scroller/dist/vue-virtual-scroller.css'

const options = {
  threshold: 0.3,
  distance: 10,
  keys: ['name', 'symbol'],
}

export default {
  components: {
    RecycleScroller,
  },
  props: {
    search: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      coinList: [],
      error: '',
      loading: true,
      tiesto: 'hello',
      fuse: null,
      searchedList: [],
    }
  },
  watch: {
    search(value) {
      if (value === '') {
        this.searchedList = this.coinList
      } else {
        this.searchedList = this.fuse.search(value).map((item) => item.item)
      }
    },
  },
  async mounted() {
    try {
      this.error = null
      this.loading = true
      const coins = await this.$axios.$get('/tickers')
      this.fuse = new Fuse(coins, options)
      this.coinList = coins
      this.searchedList = coins
      this.loading = false
    } catch (error) {
      this.loading = false
    }
  },
}
</script>

<style scoped>
.scroller {
  height: 100%;
  width: calc(100vw - 32px);
  margin: auto;
}
</style>
