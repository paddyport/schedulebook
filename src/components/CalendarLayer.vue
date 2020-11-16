<template>
  <div id="Calendar" class="calendar">
    <div class="header">
      <GenerIcnbtn
        :btn-flg="true"
        :btn-str="'メニュー'"
        :btn-cls="'def nml cng'"
        @icnbtn-click="oclick">
      </GenerIcnbtn>
      <GenerHead
        :head-str="currentYyMm.yy+'年'+currentYyMm.mm+'月'">
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
        :icon-flg="true"
        :btn-flg="true"
        :btn-str="'新規作成'"
        :btn-cls="'def nml scd'"
        @txtbtn-click="openAnewScd">
      </GenerTxtbtn>
      <GenerTxtbtn
        :icon-flg="true"
       :btn-flg="true"
        :btn-str="'新規作成'"
        :btn-cls="'def nml tsk'"
        @txtbtn-click="openAnewTsk">
      </GenerTxtbtn>
    </div>
  </div>
</template>

<script>
import GenerHead from './GenerHead'
import GenerIcnbtn from './GenerIcnbtn'
import GenerTxtbtn from './GenerTxtbtn'

export default {
// GP component
  name: 'CalendarTable',
  props: {
    currentYyMm: Object,
    currentDatesArr: Array,
  },
  components: {
    GenerHead,
    GenerIcnbtn,
    GenerTxtbtn,
  },
  methods: {
    oclick(e) {
      console.log(e);
    },
    openAnewScd() {
      this.$emit("an-open-now-anew-scd");
    },
    openAnewTsk() {
      this.$emit("an-open-now-anew-tsk");
    }
  },
}
</script>

<style scoped>
@import "../css/calendar.css";
</style>