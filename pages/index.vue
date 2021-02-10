<template>
  <div class="container">
    <TheHeroText />
    <TheSearchBar @update="setSearch($event)" />
    <CoinList :search="search" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      search: '',
    }
  },
  methods: {
    setSearch(value) {
      this.search = value
    },
  },
}
</script>
