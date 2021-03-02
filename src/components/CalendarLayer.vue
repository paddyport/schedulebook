<template>
  <div id="Calendar" class="calendar">
    <div class="header">
      <GenerIcnbtn
        :btn-flg="true"
        :btn-cls="'def nml cng'"
        @icnbtn-click="switchMonthList">
      </GenerIcnbtn>
      <div 
        id="MonthList"
        :class="['monthlist', monthListFlg ? 'isActive' : '']">
        <ul>
          <li 
            v-for="(ml, mlidx) in monthListArr"
            :class="[currentYyMm.yy==ml.yy&&currentYyMm.mm==ml.mm ? 'isCurrent' : '']"
            :key="mlidx">
            <a @click="clickMonth">
              <span>{{ ml.yy }}年{{ ml.mm+1 }}月</span>
              <small v-if="ml.prj.length">{{ ml.prj.length }}</small>
            </a>
          </li>
        </ul>
      </div>
      <GenerHead
        :head-str="currentYyMm.yy+'年'+(Number(currentYyMm.mm)+1)+'月'">
      </GenerHead>
    </div>
    <div class="body">
      <div class="month">
        <div v-for="(cd, cdidx) in currentDatesArr" class="day" :key="cdidx">
          <a 
            v-if="cd.date"
            class="content"
            @click="getDate">
            <div 
              v-for="(cp, cpidx) in cd.prj"
              :class="['prjbar', cp.start===cd.time ? 'isFirst' : '', cp.end===cd.time ? 'isLast' : '']"
              :key="cpidx">
            </div>
            <p v-if="cd.prj.length>3" class="note">＋{{ cd.prj.length-3 }}</p>
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
import GenerHead from './GenerHead'
import GenerIcnbtn from './GenerIcnbtn'
import GenerTxtbtn from './GenerTxtbtn'

export default {
  name: 'CalendarTable',
  props: {
    currentYyMm: Object,
    currentDatesArr: Array,
    monthListArr: Array,
  },
  data() {
    return {
      monthListFlg: false,
    }
  },
  components: {
    GenerHead,
    GenerIcnbtn,
    GenerTxtbtn,
  },
  methods: {
    switchMonthList() {
      this.monthListFlg = this.monthListFlg ? false : true;
    },
    clickMonth(e) {
      const btn = e.target.parentNode,
        arr = [].slice.call(btn.parentNode.childNodes),
        lis = arr.filter(li => !li.nodeName.match(/text/)),
        idx = lis.findIndex(list => list==btn),
        yy = this.monthListArr[idx].yy,
        mm = this.monthListArr[idx].mm;
      this.monthListFlg = false;
      this.$emit("ap-change-month", {year: yy, month: mm});
    },
    getDate(e) {
      this.$emit("ap-≈shown-loader");
      const btn = e.target.parentNode,
        arr = [].slice.call(btn.parentNode.childNodes),
        lis = arr.filter(li => !li.nodeName.match(/text/)),
        idx = lis.findIndex(list => list==btn),
        time = new Date(this.currentYyMm.yy, this.currentYyMm.mm, this.currentDatesArr[idx].date).getTime();
      this.$emit("ap-show-date-list", time, time);
    },
    openAnewPrj() {
      this.$emit("ap-shown-loader");
      this.$emit("ap-open-current-anew-prj");
    },
    openAnewTsk() {
      this.$emit("ap-shown-loader");
      this.$emit("ap-open-current-anew-tsk");
    }
  },
}
</script>

<style scoped>
@import "../css/calendar.css";
</style>