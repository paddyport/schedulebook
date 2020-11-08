<template>
  <div class="body">
    <div class="month">
      <div v-for="(cd, cdidx) in currentDatesArr" class="day" :key="cdidx">
        <a class="content" :data-date="cd.date">
        </a>
        <div v-if="cd.date" class="index">{{ cd.date }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CalendarTable',
  props: {
    currentDatesArr: Array,
  },
}
</script>