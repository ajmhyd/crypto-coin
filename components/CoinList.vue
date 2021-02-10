<template>
  <div class="contianer mx-auto flex justify-center mt-8">
    <ul v-if="coinList.length">
      <div
        class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg"
      >
        <table class="min-w-full divide-y divide-gray-200">
          <thead class="bg-gray-50">
            <tr class="flex justify-between">
              <th
                scope="col"
                class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
              >
                Name
              </th>
              <th
                scope="col"
                class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
              >
                Price (+/- 1h)
              </th>
              <th
                scope="col"
                class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
              >
                Rank
              </th>
            </tr>
          </thead>
        </table>
      </div>
      <div class="h-96">
        <RecycleScroller
          v-slot="{ item }"
          class="scroller"
          :items="coinList"
          :item-size="64"
          key-field="id"
        >
          <div class="divide-y" @click="$emit('updateCoin', item)">
            <CoinListItem :coin="item" @updateCoin="updateCoin($event)" />
          </div>
        </RecycleScroller>
      </div>
    </ul>
    <div v-else class="rounded-md bg-red-50 p-4 max-w-lg">
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
            No coins matched your search. Please try again.
          </h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { RecycleScroller } from 'vue-virtual-scroller'
import 'vue-virtual-scroller/dist/vue-virtual-scroller.css'

export default {
  components: {
    RecycleScroller,
  },
  props: {
    coinList: {
      type: Array,
      required: true,
      default() {
        return []
      },
    },
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
