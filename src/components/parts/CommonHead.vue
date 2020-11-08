<template>
  <h1>{{ headStr }}</h1>
</template>

<script>
export default {
  name: 'CommonHead',
  props: {
    headStr: String,
  },
}
</script>