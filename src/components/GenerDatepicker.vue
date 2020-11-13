<template>
<div class="datepicker">
  <div class="heads">
    <button
      type="button"
      :class="['prv', datingYyMmDd.yy==(nowYyMmDd.yy-1)&&datingYyMmDd.mm==(nowYyMmDd.mm+1) ? 'isNoActive' : '']"
      @click="shiftMonth">
    </button>
    <p>{{ datingYyMmDd.yy }}年{{ datingYyMmDd.mm+1 }}月</p>
    <button
      type="button"
      :class="['nxt', datingYyMmDd.yy==(nowYyMmDd.year+1)&&datingYyMmDd.mm==(nowYyMmDd.mm-1) ? 'isNoActive' : '']"
      @click="shiftMonth">
    </button>
  </div>
  <div class="dates">
    <div class="month">
      <div v-for="(da, daidx) in datingArr" class="day" v-bind:key="daidx">
        <a
          v-if="da"
          :class="[startTime<da&&da<endTime ? 'isBet' : '', startTime==da||endTime==da ? 'isFixed' : '', startTime==da ? 'isStart' : '', endTime==da ? 'isEnd' : '']"
          @click="selectDating"
        >{{ new Date(da).getDate() }}</a>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: "GenerDatepicker",
  props: {
    nowYyMmDd: Object,
    markYyMmDd: Object,
    startTime: Number,
    endTime: Number,
  },
  data() {
    return {
      weekLen: 7,
      datingYyMmDd: this.markYyMmDd,
      datingArr: [],
      datingArrCnt: 0,
      datingFirstDay: 0,
      datingWeeks: 0,
    }
  },
  created: function(){
    this.getDating(this.markYyMmDd);
  },
  methods: {
    getDating(yymmdd) {
      console.log("a");
      this.datingYyMmDd = yymmdd ? yymmdd : this.datingYyMm;
			this.datingFirstDay = new Date(yymmdd.yy, yymmdd.mm).getDay();
			this.datingArrCnt = new Date(parseInt(yymmdd.yy, 10), parseInt(yymmdd.mm+1, 10), 0).getDate();
			this.datingWeeks = Math.ceil((this.datingFirstDay+this.datingArrCnt)/this.weekLen);
      this.datingArr = [];
			for(let w=0;w<this.datingWeeks;w++) {
				for(let d=0;d<this.weekLen;d++) {
          let _d = !w&&d<this.datingFirstDay ? null : w*this.weekLen+(d+1)-this.datingFirstDay,
            _t = _d&&this.datingArrCnt<_d ? null : new Date(yymmdd.yy, yymmdd.mm, _d).getTime();
          this.datingArr.push(_t);
				}
      }
    },
    shiftMonth() {

    },
    selectDating(e) {
      console.log(e);
      this.$emit("select-date");
    },
  },
}
</script>

<style scoped>
@import "../css/datepicker.css";
</style>