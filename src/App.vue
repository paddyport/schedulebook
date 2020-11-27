<template>
  <div id="App"
    :class="[
      deviceType,
      2<=now.getMonth()&&now.getMonth()<5 ? 'spr' : 
      5<=now.getMonth()&&now.getMonth()<8 ? 'smm' : 
      8<=now.getMonth()&&now.getMonth()<11 ? 'atm' : 'wnt']">
    <CalendarLayer
      :current-yy-mm="{yy: currentYear, mm: currentMonth}"
      :current-dates-arr="currentDatesArr"
      @an-shown-loader="shownLoader"
      @an-open-now-anew-scd="openNowAnewScd"
      @an-open-now-anew-tsk="openNowAnewTsk">
    </CalendarLayer>
    <AnewLayerScd
      v-if="anewScdFlg"
      :now-year="now.getFullYear()"
      :now-month="now.getMonth()"
      :now-date="now.getDate()"
      :ctg-name="ctgName"
      :mark-year="markYear"
      :mark-month="markMonth"
      :mark-date="markDate"
      :lbl-arr="lblArr"
      @an-close-anew="closeAnew">
    </AnewLayerScd>
    <AnewLayerTsk
      v-if="anewTskFlg"
      :now-year="now.getFullYear()"
      :now-month="now.getMonth()"
      :now-date="now.getDate()"
      :ctg-name="ctgName"
      :mark-year="markYear"
      :mark-month="markMonth"
      :mark-date="markDate"
      :scd-arr="scdArr"
      :lbl-arr="lblArr"
      @an-close-anew="closeAnew">
    </AnewLayerTsk>
    <LoaderLayer v-if="loaderFlg"></LoaderLayer>
  </div>
</template>

<script>
import Dexie from 'dexie'
import CalendarLayer from './components/CalendarLayer'
import AnewLayerScd from './components/AnewLayerScd'
import AnewLayerTsk from './components/AnewLayerTsk'
import LoaderLayer from './components/LoaderLayer'

export default {
  name: '#App',
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
      scdArr: [],
      lblArr: [],
      anewScdFlg: false,
      anewTskFlg: false,
      ctgName: "",
      markYear: 0,
      markMonth: 0,
      markDate: 0,
    }
  },
  components: {
    CalendarLayer,
    AnewLayerScd,
    AnewLayerTsk,
    LoaderLayer,
  },
  created: function(){
    this.checkDevice();
    this.createDB();
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
		checkDevice() {
      const ua = navigator.userAgent.toLowerCase();
      console.log(ua);
			if(ua.indexOf("iphone")>0 || ua.indexOf("ipod")>0 || ua.indexOf("android")>0 && ua.indexOf("mobile")>0) {
				this.deviceType = "isTD";
			} else if (ua.indexOf("ipad")>0 || ua.indexOf("android")>0){
				this.deviceType = "isTD";
			} else {
				this.deviceType = "isMD";
			}
		},
    createDB() {
			this.db = new Dexie(this.dbName);
      this.createTable();
		},
		createTable() {
			this.db.version(1).stores({
				scd: "++sid, lid, start, end, notice, member, title, memo",
        tsk: "++tid, lid, sid, date, loop, notice, priority, title, memo",
        lbl: "++lid, color, title",
      });
      // scd: sid, lid, start, end, notice, member, title, memo, 
      // tsk: tid, lid, sid, date, loop, notice, priority(3以下), title, memo
      // lbl: lid, color, title
      // this.testAddDB();
      // console.log(this.db);
    },
    testAddDB() {
      // test
      this.db.scd.put({
        sid: 1,
        lid: 1,
        start: new Date(2020,11,30).getTime(),
        end: new Date(2020,11,30).getTime(),
        notice: true,
        member: [],
        title: "徒然なるまゝに",
        memo: ""
      });
      this.db.scd.put({
        sid: 2,
        lid: 2,
        start: new Date(2020,11,15).getTime(),
        end: new Date(2020,11,16).getTime(),
        notice: false,
        member: ["aaa"],
        title: "日暮らし",
        memo: "あやしうこそものぐるほしけれ。"
      });
      this.db.scd.put({
        sid: 3,
        lid: 2,
        start: new Date(2020,12,1).getTime(),
        end: new Date(2020,12,5).getTime(),
        notice: false,
        member: [],
        title: "あやしうこそ",
        memo: ""
      });
      this.db.tsk.put({
        tid: 1,
        lid: 2,
        sid: 2,
        date: new Date(2020,11,14).getTime(),
        loop: false,
        notice: true,
        priority: 3,
        title: "あやしう",
        memo: "",
      });
      this.db.tsk.put({
        tid: 1,
        lid: 3,
        sid: null,
        date: 20,
        loop: "month",
        notice: true,
        priority: 2,
        title: "硯",
        memo: "",
      });
      this.db.lbl.put({
        lid: 1,
        color: "#fcf4b7",
        title: "竹の園生",
      });
      this.db.lbl.put({
        lid: 2,
        color: "#fa87a5",
        title: "因幡国",
      });
      this.db.lbl.put({
        lid: 3,
        color: "#2a668c",
        title: "末葉(すゑば)",
      });
    },
		getScdAllData() {
			const that = this;
			return new Promise(function(resolve){
				that.db.scd.toArray().then((list) => {
					resolve(list);
				});
			});
		},
		getLblAllData() {
			const that = this;
			return new Promise(function(resolve){
				that.db.lbl.toArray().then((list) => {
					resolve(list);
				});
			});
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
    async openNowAnewScd() {
      this.openAnew("scd", {yy: this.now.getFullYear(), mm: this.now.getMonth(), dd: this.now.getDate()});
      this.lblArr = await this.getLblAllData();
      this.anewScdFlg = true;
      this.hiddenLoader();
    },
    async openNowAnewTsk() {
      this.openAnew("tsk", {yy: this.now.getFullYear(), mm: this.now.getMonth(), dd: this.now.getDate()});
      this.scdArr = await this.getScdAllData();
      this.lblArr = await this.getLblAllData();
      this.anewTskFlg = true;
      this.hiddenLoader();
    },
    openAnew(ctg, yymmdd) {
      this.ctgName = ctg;
      this.markYear = yymmdd.yy;
      this.markMonth = yymmdd.mm;
      this.markDate = yymmdd.dd;
    },
    closeAnew() {
      this.ctgName = "";
      this.anewScdFlg = false;
      this.anewTskFlg = false;
    },
  },
}
</script>

<style>
@import "./css/app.css";
</style>
