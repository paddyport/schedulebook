<template>
  <div id="App"
    :class="[
      deviceType,
      2<=now.getMonth()&&now.getMonth()<5 ? 'spr' : 
      5<=now.getMonth()&&now.getMonth()<8 ? 'smm' : 
      8<=now.getMonth()&&now.getMonth()<11 ? 'atm' : 
      'wnt']">
    <CalendarLayer
      :now-yy-mm="{yy: now.getFullYear(), mm: now.getMonth()}"
      :current-yy-mm="{yy: currentYear, mm: currentMonth}"
      :current-dates-arr="currentDatesArr"
      @ap-shown-loader="shownLoader"
      @ap-open-now-anew-prj="openNowAnewPrj"
      @ap-open-now-anew-tsk="openNowAnewTsk">
    </CalendarLayer>
    <AnewLayerPrj
      v-if="anewPrjFlg"
      :now-year="now.getFullYear()"
      :now-month="now.getMonth()"
      :now-date="now.getDate()"
      :ctg-name="ctgName"
      :mark-year="markYear"
      :mark-month="markMonth"
      :mark-date="markDate"
      :mmb-arr="mmbArr"
      @ap-anew-prj="anewPrj"
      @ap-close-anew="closeAnew">
    </AnewLayerPrj>
    <AnewLayerTsk
      v-if="anewTskFlg"
      :now-year="now.getFullYear()"
      :now-month="now.getMonth()"
      :now-date="now.getDate()"
      :ctg-name="ctgName"
      :mark-year="markYear"
      :mark-month="markMonth"
      :mark-date="markDate"
      :prj-arr="prjArr"
      :lbl-arr="lblArr"
      @ap-close-anew="closeAnew">
    </AnewLayerTsk>
    <LoaderLayer v-if="loaderFlg"></LoaderLayer>
  </div>
</template>

<script>
import Dexie from 'dexie'
import CalendarLayer from './components/CalendarLayer'
import AnewLayerPrj from './components/AnewLayerPrj'
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
      prjArr: [],
      mmbArr: [],
      lblArr: [],
      anewPrjFlg: false,
      anewTskFlg: false,
      ctgName: "",
      markYear: 0,
      markMonth: 0,
      markDate: 0,
    }
  },
  components: {
    CalendarLayer,
    AnewLayerPrj,
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
				prj: "++pid, start, end, *member, title",
        tsk: "++tid, lid, pid, *tids, *member, start, end, priority, title",
        lbl: "++lid, color, title",
        mmb: "++mid, name, authority, address",
      });
      // DB連携後のイメージ
      // グループごとに分かれ、グループ内で案件・メンバーを設定（メンバーを増やす時は招待）
      // （メンバー招待してからでないと案件・タスクに登録できない）
      // 案件内でタスクを設定
      // prj: pid, start, end, member, title, memo(検索なし),
      // tsk: tid, lid, pid, tids, member, start, end, notice(検索なし), priority(3以下), title, memo(検索なし)
      // mmb: mid, name, authority(2以下), icon(検索なし), address, pass(検索なし)
      // メンバーは各自で変更する要素が多い（↓参照）が、活用できるのはDB連携が可能になってから
      // → authority: タスク、案件の変更・登録の権限
      //              0（全権（案件削除など））、
      //              1（タスク・案件登録変更、タスク削除、メンバー招待）、
      //              2（タスクの状態変更のみ）
      // → icon: サムネイル（各自で変更）
      // → address: ログイン・通知・グループへの招待に使用（各自で変更）
      // → pass: ログインに使用（各自で変更）
      // lbl: lid, color, title
      // this.testAddDB();
      // console.log(this.db);
    },
    testAddDB() {
      // test
      this.db.prj.put({
        pid: 1,
        start: new Date(2020,11,30).getTime(),
        end: new Date(2020,11,30).getTime(),
        member: [1, 2, 3, 4],
        title: "徒然なるまゝに",
        memo: ""
      });
      this.db.prj.put({
        pid: 2,
        start: new Date(2021,11,15).getTime(),
        end: new Date(2021,11,16).getTime(),
        member: [1, 4],
        title: "日暮らし",
        memo: "あやしうこそものぐるほしけれ。"
      });
      this.db.prj.put({
        pid: 3,
        start: new Date(2021,12,1).getTime(),
        end: new Date(2021,12,5).getTime(),
        member: [1, 2, 4],
        title: "あやしうこそ",
        memo: ""
      });
      this.db.tsk.put({
        tid: 1,
        lid: 2,
        pid: 2,
        tids: [2],
        start: new Date(2021,11,15).getTime(),
        end: new Date(2021,11,15).getTime(),
        member: [1, 4],
        notice: true,
        priority: 3,
        title: "あやしう",
        memo: "",
      });
      this.db.tsk.put({
        tid: 2,
        lid: 3,
        pid: 3,
        tids: [],
        start: new Date(2021,12,1).getTime(),
        end: new Date(2021,12,2).getTime(),
        member: [2, 4],
        notice: true,
        priority: 2,
        title: "硯",
        memo: "",
      });
      this.db.mmb.put({
        mid: 1,
        name: "山田太郎",
        authority: 2,
        address: "example1@gmail.com",
        icon: "/assets/images/icon01.png"
      });
      this.db.mmb.put({
        mid: 2,
        name: "小林太郎",
        authority: 1,
        address: "example2@gmail.com",
        icon: "/assets/images/icon02.png"
      });
      this.db.mmb.put({
        mid: 3,
        name: "鈴木一郎",
        authority: 0,
        address: "example3@gmail.com",
        icon: "/assets/images/icon03.png"
      });
      this.db.mmb.put({
        mid: 4,
        name: "鈴木二郎",
        authority: 0,
        address: "example4@gmail.com",
        icon: "/assets/images/icon04.png"
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
		getPrjAllData() {
			const that = this;
			return new Promise(function(resolve){
				that.db.prj.toArray().then((list) => {
					resolve(list);
				});
			});
		},
    getMmbAllData() {
      const that = this;
			return new Promise(function(resolve){
				that.db.mmb.toArray().then((list) => {
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
			// let pt = this.current.getTime(),
			// 	ct = new Date(yymm.year, yymm.month).getTime(),
			// 	flg = Boolean(pt==ct);
			this.current = new Date(yymm.year, yymm.month);
			this.currentYear = this.current.getFullYear();
			this.currentMonth = this.current.getMonth();
			this.setCalendar(yymm);
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
    async openNowAnewPrj() {
      this.openAnew("prj", {yy: this.now.getFullYear(), mm: this.now.getMonth(), dd: this.now.getDate()});
      this.mmbArr = await this.getMmbAllData();
      this.anewPrjFlg = true;
      this.hiddenLoader();
    },
    async openNowAnewTsk() {
      this.openAnew("tsk", {yy: this.now.getFullYear(), mm: this.now.getMonth(), dd: this.now.getDate()});
      this.prjArr = await this.getPrjAllData();
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
    async anewPrj(...args) {
      const that = this,
        [title, start, end, member, memo] = args,
        prjLen = await that.getPrjAllData(),
        pid = prjLen.length+1;
      that.db.prj.put({
        pid: pid,
        start: start,
        end: end,
        member: member,
        title: title,
        memo: memo
      });
    },
    closeAnew() {
      this.ctgName = "";
      this.anewPrjFlg = false;
      this.anewTskFlg = false;
    },
  },
}
</script>

<style>
@import "./css/app.css";
</style>
