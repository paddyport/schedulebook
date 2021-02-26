<template>
  <h1>{{ headStr }}</h1>
</template>

<script>
export default {
  name: 'GenerHead',
  props: {
    headStr: String,
  },
}
</script>