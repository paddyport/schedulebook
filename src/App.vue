<template>
  <div id="App">
    <calendar-container
      :currentYYMM="{yy: currentYear, mm: currentMonth}"
      :currentDatesArr="currentDatesArr">
    </calendar-container>
  </div>
</template>

<script>
import Dexie from 'dexie'
import CalendarContainer from './components/calendar/CalendarContainer'

export default {
  name: 'App',
  data() {
    return {
      db: "",
      dbName: "schedulebookDB",
      now: "",
      current: "",
      currentYear: 0,
      currentMonth: 0,
      currentDatesArr: [],
      currentDatesCnt: 0,
      currentWeeks: 0,
      weekLen: 7,
    }
  },
  components: {
    CalendarContainer,
  },
  created: function(){
    this.createDB();
		this.createTable();
    this.setToday();
  },
  methods: {
    zeroPad(num, len) {
      return ('0000'+num).slice(-len);
    },
    getCurrentDates(y, m) {
      return new Date(parseInt(y, 10), parseInt(m+1, 10), 0).getDate();
    },
    createDB() {
			this.db = new Dexie(this.dbName);
		},
		createTable() {

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
      console.log(this.currentDatesArr);
		},
  },
}
</script>

<style>
@import "./css/app.css";
</style>
