<template>
  <div class="mt-8">
    <h2
      class="text-4xl tracking-tight font-extrabold text-gray-900 sm:text-5xl md:text-6xl lg:text-5xl xl:text-6xl"
    >
      <span class="block xl:inline">{{ coin.name }}</span>
      <span class="block text-green-600 xl:inline">{{ coin.symbol }}</span>
    </h2>
    <p class="ml-2 mt-1 text-sm text-gray-500 truncate">24h change</p>
    <dl
      class="mt-5 grid grid-cols-1 rounded-lg bg-white overflow-hidden shadow divide-y divide-gray-200 md:grid-cols-3 md:divide-y-0 md:divide-x"
    >
      <div>
        <div class="px-4 py-5 sm:p-6">
          <dt class="text-base font-normal text-gray-900">Price</dt>
          <dd class="mt-1 flex justify-between items-baseline md:block lg:flex">
            <div
              class="flex items-baseline text-2xl font-semibold text-green-600"
            >
              {{ formatDollars(coin.quotes.USD.price) }}
            </div>

            <div
              v-if="coin.quotes.USD.percent_change_24h < 0"
              class="inline-flex items-baseline px-2.5 py-0.5 rounded-full text-sm font-medium bg-red-100 text-red-800"
            >
              <svg
                class="-ml-1 mr-0.5 flex-shrink-0 self-center h-5 w-5 text-red-500"
                fill="currentColor"
                viewBox="0 0 20 20"
                aria-hidden="true"
              >
                <path
                  fill-rule="evenodd"
                  d="M14.707 10.293a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 111.414-1.414L9 12.586V5a1 1 0 012 0v7.586l2.293-2.293a1 1 0 011.414 0z"
                  clip-rule="evenodd"
                />
              </svg>
              <span class="sr-only"> Decreased by </span>
              {{ coin.quotes.USD.percent_change_24h }}%
            </div>
            <div
              v-else
              class="inline-flex items-baseline px-2.5 py-0.5 rounded-full text-sm font-medium bg-green-100 text-green-800 md:mt-2 lg:mt-0"
            >
              <svg
                class="-ml-1 mr-0.5 flex-shrink-0 self-center h-5 w-5 text-green-500"
                fill="currentColor"
                viewBox="0 0 20 20"
                aria-hidden="true"
              >
                <path
                  fill-rule="evenodd"
                  d="M5.293 9.707a1 1 0 010-1.414l4-4a1 1 0 011.414 0l4 4a1 1 0 01-1.414 1.414L11 7.414V15a1 1 0 11-2 0V7.414L6.707 9.707a1 1 0 01-1.414 0z"
                  clip-rule="evenodd"
                />
              </svg>
              <span class="sr-only"> Increased by </span>
              {{ coin.quotes.USD.percent_change_24h }}%
            </div>
          </dd>
        </div>
      </div>

      <div>
        <div class="px-4 py-5 sm:p-6">
          <dt class="text-base font-normal text-gray-900">Volume</dt>
          <dd class="mt-1 flex justify-between items-baseline md:block lg:flex">
            <div
              class="flex items-baseline text-2xl font-semibold text-green-600"
            >
              {{ formatDollars(coin.quotes.USD.volume_24h) }}
            </div>

            <div
              v-if="coin.quotes.USD.volume_24h_change_24h < 0"
              class="inline-flex items-baseline px-2.5 py-0.5 rounded-full text-sm font-medium bg-red-100 text-red-800"
            >
              <svg
                class="-ml-1 mr-0.5 flex-shrink-0 self-center h-5 w-5 text-red-500"
                fill="currentColor"
                viewBox="0 0 20 20"
                aria-hidden="true"
              >
                <path
                  fill-rule="evenodd"
                  d="M14.707 10.293a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 111.414-1.414L9 12.586V5a1 1 0 012 0v7.586l2.293-2.293a1 1 0 011.414 0z"
                  clip-rule="evenodd"
                />
              </svg>
              <span class="sr-only"> Decreased by </span>
              {{ coin.quotes.USD.volume_24h_change_24h }}%
            </div>
            <div
              v-else
              class="inline-flex items-baseline px-2.5 py-0.5 rounded-full text-sm font-medium bg-green-100 text-green-800 md:mt-2 lg:mt-0"
            >
              <svg
                class="-ml-1 mr-0.5 flex-shrink-0 self-center h-5 w-5 text-green-500"
                fill="currentColor"
                viewBox="0 0 20 20"
                aria-hidden="true"
              >
                <path
                  fill-rule="evenodd"
                  d="M5.293 9.707a1 1 0 010-1.414l4-4a1 1 0 011.414 0l4 4a1 1 0 01-1.414 1.414L11 7.414V15a1 1 0 11-2 0V7.414L6.707 9.707a1 1 0 01-1.414 0z"
                  clip-rule="evenodd"
                />
              </svg>
              <span class="sr-only"> Increased by </span>
              {{ coin.quotes.USD.volume_24h_change_24h }}%
            </div>
          </dd>
        </div>
      </div>

      <div>
        <div class="px-4 py-5 sm:p-6">
          <dt class="text-base font-normal text-gray-900">Market Cap</dt>
          <dd class="mt-1 flex justify-between items-baseline md:block lg:flex">
            <div
              class="flex items-baseline text-2xl font-semibold text-green-600"
            >
              {{ formatDollars(coin.quotes.USD.market_cap) }}
            </div>

            <div
              v-if="coin.quotes.USD.market_cap_change_24h < 0"
              class="inline-flex items-baseline px-2.5 py-0.5 rounded-full text-sm font-medium bg-red-100 text-red-800"
            >
              <svg
                class="-ml-1 mr-0.5 flex-shrink-0 self-center h-5 w-5 text-red-500"
                fill="currentColor"
                viewBox="0 0 20 20"
                aria-hidden="true"
              >
                <path
                  fill-rule="evenodd"
                  d="M14.707 10.293a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 111.414-1.414L9 12.586V5a1 1 0 012 0v7.586l2.293-2.293a1 1 0 011.414 0z"
                  clip-rule="evenodd"
                />
              </svg>
              <span class="sr-only"> Decreased by </span>
              {{ coin.quotes.USD.market_cap_change_24h }}%
            </div>
            <div
              v-else
              class="inline-flex items-baseline px-2.5 py-0.5 rounded-full text-sm font-medium bg-green-100 text-green-800 md:mt-2 lg:mt-0"
            >
              <svg
                class="-ml-1 mr-0.5 flex-shrink-0 self-center h-5 w-5 text-green-500"
                fill="currentColor"
                viewBox="0 0 20 20"
                aria-hidden="true"
              >
                <path
                  fill-rule="evenodd"
                  d="M5.293 9.707a1 1 0 010-1.414l4-4a1 1 0 011.414 0l4 4a1 1 0 01-1.414 1.414L11 7.414V15a1 1 0 11-2 0V7.414L6.707 9.707a1 1 0 01-1.414 0z"
                  clip-rule="evenodd"
                />
              </svg>
              <span class="sr-only"> Increased by </span>
              {{ coin.quotes.USD.market_cap_change_24h }}%
            </div>
          </dd>
        </div>
      </div>
    </dl>
  </div>
</template>

<script>
export default {
  props: {
    coin: {
      type: Object,
      required: true,
      default() {
        return {
          symbol: '',
          name: '',
        }
      },
    },
  },
  computed: {
    price() {
      return new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: 'USD',
        minimumSignificantDigits: 10,
      }).format(this.coin.quotes.USD.price)
    },
  },
  methods: {
    formatDollars(value) {
      return new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: 'USD',
        minimumSignificantDigits: 10,
      }).format(value)
    },
  },
}
</script>
