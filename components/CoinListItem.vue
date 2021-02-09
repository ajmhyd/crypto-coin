<template>
  <h1>{{ coin.symbol }}</h1>
</template>

<script>
export default {
  props: {
    coin: Object,
  },
}
</script>
