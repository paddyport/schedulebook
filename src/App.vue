<template>
  <div id="App" :class="deviceType">
    <CalendarLayer
      :current-yy-mm="{yy: currentYear, mm: currentMonth}"
      :current-dates-arr="currentDatesArr"
      @an-open-now-anew="openNowAnew">
    </CalendarLayer>
    <AnewLayer
      v-if="anewFlg"
      :ctg-name="ctgName"
      :mark-yy-mm-dd="{yy: markYear, mm: markMonth, dd: markDate}"
      :markYear="markYear"
      :markMonth="markMonth"
      :markDate="markDate"
      @an-close-anew="closeAnew">
    </AnewLayer>
    <LoaderLayer v-if="loaderFlg"></LoaderLayer>
  </div>
</template>

<script>
import Dexie from 'dexie'
import CalendarLayer from './components/CalendarLayer'
import AnewLayer from './components/AnewLayer'
import LoaderLayer from './components/LoaderLayer'

export default {
  name: 'App',
  data() {
    return {
      loaderFlg: true,
      db: "",
      dbName: "schedulebookDB",
      deviceType: "",
      now: "",
      current: "",
      currentYear: 0,
      currentMonth: 0,
      currentDatesArr: [],
      currentDatesCnt: 0,
      currentWeeks: 0,
      weekLen: 7,
      anewFlg: false,
      ctgName: "",
      markYear: 0,
      markMonth: 0,
      markDate: 0,
    }
  },
  components: {
    CalendarLayer,
    AnewLayer,
    LoaderLayer,
  },
  created: function(){
    this.checkDevice();
    this.createDB();
		this.createTable();
    this.setToday();
    this.hiddenLoader();
  },
  methods: {
    zeroPad(num, len) {
      return ('0000'+num).slice(-len);
    },
    getCurrentDates(y, m) {
      return new Date(parseInt(y, 10), parseInt(m+1, 10), 0).getDate();
    },
    shownLoader() {
      this.loaderFlg = true;
    },
    hiddenLoader() {
      this.loaderFlg = false;
    },
    createDB() {
			this.db = new Dexie(this.dbName);
		},
		createTable() {

    },
		checkDevice() {
			const ua = navigator.userAgent.toLowerCase();
			if(ua.indexOf("iphone")>0 || ua.indexOf("ipod")>0 || ua.indexOf("android")>0 && ua.indexOf("mobile")>0) {
				this.deviceType = "isTD";
			} else if (ua.indexOf("ipad")>0 || ua.indexOf("android")>0){
				this.deviceType = "isTD";
			} else {
				this.deviceType = "isMD";
			}
		},
    setToday() {
      this.now = new Date();
			this.now = new Date(this.now.getFullYear(), this.now.getMonth(), this.now.getDate());
			this.nowTime = this.now.getTime();
			this.current = new Date(this.now.getFullYear(), this.now.getMonth());
			this.setCurrent({year: this.now.getFullYear(), month: this.now.getMonth()});
    },
    setCurrent(yymm) {
			let pt = this.current.getTime(),
				ct = new Date(yymm.year, yymm.month).getTime(),
				flg = Boolean(pt==ct);
			this.current = new Date(yymm.year, yymm.month);
			this.currentYear = this.current.getFullYear();
			this.currentMonth = this.current.getMonth();
			this.setCalendar(yymm);
			if(!flg) {
				// this.switchChangeMonth();
			}
    },
    setCalendar(yymm) {
			// this.changeMonthObj = {};
			this.currentFirstDay = new Date(yymm.year, yymm.month).getDay();
			this.currentDatesCnt = this.getCurrentDates(yymm.year, yymm.month);
			this.currentWeeks = Math.ceil((this.currentFirstDay+this.currentDatesCnt)/this.weekLen);
			this.setCalendarArr(yymm);
			// this.setDataCtg(yymm);
		},
    setCalendarArr(yymm) {
			this.currentDatesArr = [];
			for(let w=0;w<this.currentWeeks;w++) {
				for(let d=0;d<this.weekLen;d++) {
					let obj,
						_d = !w&&d<this.currentFirstDay ? null : w*this.weekLen+(d+1)-this.currentFirstDay,
						_t = new Date(yymm.year, yymm.month, _d).getTime();
					_d = _d&&this.currentDatesCnt<_d ? null : _d;
					obj = {
						"date": _d,
						"time": _t,
					};
					this.currentDatesArr.push(obj);
				}
      }
    },
    openNowAnew(ctg) {
      this.openAnew(ctg, {yy: this.now.getFullYear(), mm: this.now.getMonth(), dd: this.now.getDate()});
    },
    openAnew(ctg, yymmdd) {
      console.log(ctg);
      this.ctgName = ctg;
      this.markYear = yymmdd.yy;
      this.markMonth = yymmdd.mm;
      this.markDate = yymmdd.dd;
      console.log(yymmdd);
      this.anewFlg = true;
    },
    closeAnew() {
      this.ctgName = "";
      this.anewFlg = false;
    },
  },
}
</script>

<style>
@import "./css/app.css";
</style>
