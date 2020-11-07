<template>
  <div id="Calendar" class="calendar">
    <calendar-table
      :currentDatesArr="currentDatesArr">
    </calendar-table>
  </div>
</template>

<script>
import CalendarTable from './CalendarTable.vue'

export default {
  name: 'CalendarContainer',
  props: {
    currentDatesArr: Array,
  },
  components: {
    CalendarTable,
  },
}
</script>

<style scoped>
@import "../../css/calendar.css";
</style>