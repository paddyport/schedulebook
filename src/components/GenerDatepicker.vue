<template>
<div class="datepicker">
  <div class="heads">
    <button
      type="button"
      :class="['prv', !prevDefTime||nowTime<prevDefTime ? '' : 'isNoActive']"
      @click="prevMonth">
    </button>
    <p>{{ datingYyMmDd.yy }}年{{ datingYyMmDd.mm+1 }}月</p>
    <button
      type="button"
      :class="['nxt', !nextDefTime||nextDefTime<nowTime ? '' : 'isNoActive']"
      @click="nextMonth">
    </button>
  </div>
  <div class="dates">
    <div class="month">
      <div v-for="(da, daidx) in datingArr" class="day" v-bind:key="daidx">
        <a
          v-if="da"
          :class="[startTime<=da&&da<=endTime ? 'isBet' : '', startTime==da ? 'isStart' : '', endTime==da ? 'isEnd' : '']"
          @click="selectDating"
        ><span>{{ new Date(da).getDate() }}</span></a>
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
      nowTime: 0,
      datingYyMmDd: this.markYyMmDd,
      prevDefTime: 0,
      nextDefTime: 0,
      datingArr: [],
      datingArrCnt: 0,
      datingFirstDay: 0,
      datingWeeks: 0,
    }
  },
  created: function(){
    this.nowTime = new Date(this.nowYyMmDd.yy, this.nowYyMmDd.mm, this.nowYyMmDd.dd).getTime();
    this.getDating(this.markYyMmDd);
  },
  methods: {
    getDating(yymmdd) {
      this.datingYyMmDd = yymmdd ? yymmdd : this.datingYyMm;
			this.datingFirstDay = new Date(yymmdd.yy, yymmdd.mm).getDay();
			this.datingArrCnt = new Date(parseInt(yymmdd.yy, 10), parseInt(yymmdd.mm+1, 10), 0).getDate();
      this.datingWeeks = Math.ceil((this.datingFirstDay+this.datingArrCnt)/this.weekLen);
      this.datingArr = [];
			for(let w=0;w<this.datingWeeks;w++) {
				for(let d=0;d<this.weekLen;d++) {
          let _d = !w&&d<this.datingFirstDay ? null : w*this.weekLen+(d+1)-this.datingFirstDay,
            _t = !_d||this.datingArrCnt<_d ? null : new Date(yymmdd.yy, yymmdd.mm, _d).getTime();
          this.datingArr.push(_t);
				}
      }
    },
    prevMonth() {
      const _ym = new Date(this.datingYyMmDd.yy, this.datingYyMmDd.mm),
        ymd = new Date(_ym.setMonth(_ym.getMonth()-1)),
        def = new Date(_ym.setMonth(_ym.getMonth()+11));
      this.prevDefTime = def.getTime();
      this.nextDefTime = 0;
      this.getDating({yy: ymd.getFullYear(), mm: ymd.getMonth(), dd: 1});
    },
    nextMonth() {
      const _ym = new Date(this.datingYyMmDd.yy, this.datingYyMmDd.mm),
        ymd = new Date(_ym.setMonth(_ym.getMonth()+1)),
        def = new Date(_ym.setMonth(_ym.getMonth()-10));
      this.nextDefTime = def.getTime();
      this.prevDefTime = 0;
      this.getDating({yy: ymd.getFullYear(), mm: ymd.getMonth(), dd: 1});
    },
    selectDating(e) {
      const btn = e.target,
        dd = Number(btn.textContent),
        _time = new Date(this.datingYyMmDd.yy, this.datingYyMmDd.mm, dd).getTime(),
        stime = _time<this.startTime ? _time : _time==this.startTime ? this.startTime+(60*60*24*1000) : this.startTime,
        etime = _time==this.endTime ? this.endTime-(60*60*24*1000) :this.startTime<_time ? _time : this.endTime;
      this.$emit("select-date", stime, etime);
    },
  },
}
</script>

<style scoped>
@import "../css/datepicker.css";
</style>