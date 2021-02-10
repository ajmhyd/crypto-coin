<template>
  <li>
    <a class="block hover:bg-gray-50 cursor-pointer">
      <div class="flex items-center px-4 py-4 sm:px-6">
        <div class="min-w-0 flex-1 flex items-center">
          <div class="min-w-0 flex-1 px-4 md:grid md:grid-cols-2 md:gap-4">
            <div>
              <p class="text-sm font-medium text-green-600 truncate">
                {{ coin.name }}
              </p>
              <p class="mt-2 flex items-center text-sm text-gray-500">
                <span class="truncate">{{ coin.symbol }}</span>
              </p>
            </div>
            <div class="hidden md:block">
              <div>
                <p class="text-sm text-gray-900">
                  {{ price }}
                </p>
                <div
                  v-if="coin.quotes.USD.percent_change_1h < 0"
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
                  {{ coin.quotes.USD.percent_change_1h }}%
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
                  {{ coin.quotes.USD.percent_change_1h }}%
                </div>
              </div>
            </div>
          </div>
        </div>
        <div>
          <p class="text-gray-500 text-sm">{{ coin.rank }}</p>
        </div>
      </div>
    </a>
  </li>
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
}
</script>
