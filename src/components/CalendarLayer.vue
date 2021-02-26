<template>
  <div id="Calendar" class="calendar">
    <div class="header">
      <GenerIcnbtn
        :btn-flg="true"
        :btn-cls="'def nml cng'"
        @icnbtn-click="oclick">
      </GenerIcnbtn>
      <MonthList
        :now-yy-mm="nowYyMm">
      </MonthList>
      <GenerHead
        :head-str="currentYyMm.yy+'年'+(Number(currentYyMm.mm)+1)+'月'">
      </GenerHead>
    </div>
    <div class="body">
      <div class="month">
        <div v-for="(cd, cdidx) in currentDatesArr" class="day" :key="cdidx">
          <a v-if="cd.date" class="content" :data-date="cd.date">
          </a>
          <div v-if="cd.date" class="index">{{ cd.date }}</div>
        </div>
      </div>
    </div>
    <div class="footer">
     <GenerTxtbtn
        :btn-flg="true"
        :btn-str="'新規作成'"
        :btn-cls="'def nml prj'"
        @txtbtn-click="openAnewPrj">
      </GenerTxtbtn>
      <GenerTxtbtn
        :btn-flg="true"
        :btn-str="'新規作成'"
        :btn-cls="'def nml tsk'"
        @txtbtn-click="openAnewTsk">
      </GenerTxtbtn>
    </div>
  </div>
</template>

<script>
import MonthList from './MonthList'
import GenerHead from './GenerHead'
import GenerIcnbtn from './GenerIcnbtn'
import GenerTxtbtn from './GenerTxtbtn'

export default {
  name: 'CalendarTable',
  props: {
    nowYyMm: Object,
    currentYyMm: Object,
    currentDatesArr: Array,
  },
  components: {
    MonthList,
    GenerHead,
    GenerIcnbtn,
    GenerTxtbtn,
  },
  methods: {
    oclick(e) {
      console.log(e);
    },
    openAnewPrj() {
      this.$emit("ap-shown-loader");
      this.$emit("ap-open-now-anew-prj");
    },
    openAnewTsk() {
      this.$emit("ap-shown-loader");
      this.$emit("ap-open-now-anew-tsk");
    }
  },
}
</script>

<style scoped>
@import "../css/calendar.css";
</style>