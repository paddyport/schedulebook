<template>
  <div id="Calendar" class="calendar">
    <calendar-header
      :currentYYMM="currentYYMM">
    </calendar-header>
    <calendar-table
      :currentDatesArr="currentDatesArr">
    </calendar-table>
  </div>
</template>

<script>
import CalendarHeader from './CalendarHeader'
import CalendarTable from './CalendarTable'

export default {
  name: 'CalendarContainer',
  props: {
    currentYYMM: Object,
    currentDatesArr: Array,
  },
  components: {
    CalendarHeader,
    CalendarTable,
  },
}
</script>

<style scoped>
@import "../../css/calendar.css";
</style>