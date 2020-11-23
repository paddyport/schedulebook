<template>
  <div id="AnewTsk" class="anew">
    <div>
      <div class="title">
        <input type="text" placeholder="新規タイトル">
      </div>
      <div class="date">
        <GenerIcnbtn
          :btn-flg="true"
          :btn-str="'選択'"
          :btn-cls="'def nml dtp'"
          @icnbtn-click="switchDatepicker">
        </GenerIcnbtn>
        <div class="res">
          <p class="start">{{ selStartYyMmDd.yy }}年{{ selStartYyMmDd.mm+1 }}月{{ selStartYyMmDd.dd }}日</p>
          <p v-if="anewStartTime!=anewEndTime" class="end">{{ selEndYyMmDd.yy }}年{{ selEndYyMmDd.mm+1 }}月{{ selEndYyMmDd.dd }}日</p>
        </div>
        <GenerDatepicker
          v-if="dateFlg"
          :now-yy-mm-dd="{yy: this.nowYear, mm: this.nowMonth, dd: this.nowDate}"
          :mark-yy-mm-dd="selEndYyMmDd"
          :start-time="anewStartTime"
          :end-time="anewEndTime"
          @select-date="selectAnewDating">
        </GenerDatepicker>
      </div>
      <div class="label">
        <GenerIcnbtn
          :btn-flg="true"
          :btn-str="'選択'"
          :btn-cls="'def nml cpr'"
          @icnbtn-click="switchColopicker">
        </GenerIcnbtn>
        <p class="res"><i v-show="anewColor" :style="{background: anewColor}"></i>{{ anewColor }}</p>
        <GenerColorpicker
          v-if="colorFlg"
          :bar-idx="anewPaletteIdx"
          :sel-color="anewColor"
          @change-swatch="changeAnewColor">
        </GenerColorpicker>
      </div>
      <div class="link">
        <GenerIcnbtn
          :btn-flg="true"
          :btn-str="'選択'"
          :btn-cls="'def nml lnk'"
          @icnbtn-click="switchScdlist">
        </GenerIcnbtn>
        <p class="res">{{ anewLinkObj.sid ? anewLinkObj.title : "－－" }}</p>
        <GenerScdlist
          v-if="linkFlg"
          :check-sid="0"
          :scd-arr="scdArr"
          :now-time="new Date(this.nowYear, this.nowMonth, this.nowDate).getTime()"
          @change-link="changeAnewScdlink">
        </GenerScdlist>
      </div>
    </div>
    <div class="footer">
      <GenerTxtbtn
        :icon-flg="true"
        :btn-flg="true"
        :btn-str="'キャンセル'"
        :btn-cls="'def nml cla'"
        @txtbtn-click="closeAnew">
      </GenerTxtbtn>
      <GenerTxtbtn
        :icon-flg="true"
        :btn-flg="true"
        :btn-str="'新規作成'"
        :btn-cls="'def nml tsk'"
        @txtbtn-click="checkAnew">
      </GenerTxtbtn>
    </div>
  </div>
</template>

<script>
import GenerDatepicker from './GenerDatepicker'
import GenerColorpicker from './GenerColorpicker'
import GenerScdlist from './GenerScdlist'
import GenerIcnbtn from './GenerIcnbtn'
// import GenerWrdbtn from './GenerWrdbtn'
import GenerTxtbtn from './GenerTxtbtn'

export default {
  name: 'AnewLayerTsk',
  props: {
    nowYear: Number,
    nowMonth: Number,
    nowDate: Number,
    ctgName: String,
    markYear: Number,
    markMonth: Number,
    markDate: Number,
    scdArr: Array,
    lblArr: Array,
  },
	data() {
		return {
      dateFlg: false,
      anewStartTime: new Date(this.markYear, this.markMonth, this.markDate).getTime(),
      selStartYyMmDd: {yy: this.markYear, mm: this.markMonth, dd: this.markDate},
      anewEndTime: new Date(this.markYear, this.markMonth, this.markDate).getTime(),
      selEndYyMmDd: {yy: this.markYear, mm: this.markMonth, dd: this.markDate},
      colorFlg: false,
      anewColor: "#fee7ed",
      anewPaletteIdx: 0,
      linkFlg: false,
      anewLinkObj: {sid: 0},
		}
  },
  components: {
    GenerDatepicker,
    GenerColorpicker,
    GenerScdlist,
    GenerIcnbtn,
    // GenerWrdbtn,
    GenerTxtbtn,
  },
  methods: {
    switchColopicker() {
      this.colorFlg = this.colorFlg ? false : true;
    },
    changeAnewColor(val, idx) {
      this.anewColor = val;
      this.anewPaletteIdx = idx;
    },
    switchDatepicker() {
      this.dateFlg = this.dateFlg ? false : true;
    },
    selectAnewDating(...args) {
      // 開始:S, 終了: E, 選択: V
      // V<S -> S = V
      // S<V -> E = V
      // 基本的に E が移動し、 S 以前が選択された時のみ S が移動する
      // S/E それぞれが選択されるとそれぞれのチェックが外れ、翌日/前日に入れ替わる
      const [stime, etime] = args;
      this.anewEndTime = etime;
      this.selEndYyMmDd.yy = new Date(this.anewEndTime).getFullYear();
      this.selEndYyMmDd.mm = new Date(this.anewEndTime).getMonth();
      this.selEndYyMmDd.dd = new Date(this.anewEndTime).getDate();
      this.anewStartTime = stime;
      this.selStartYyMmDd.yy = new Date(this.anewStartTime).getFullYear();
      this.selStartYyMmDd.mm = new Date(this.anewStartTime).getMonth();
      this.selStartYyMmDd.dd = new Date(this.anewStartTime).getDate();
    },
    switchScdlist() {
      this.linkFlg = this.linkFlg ? false : true;
    },
    changeAnewScdlink(id) {
      for(let obj of this.scdArr) {
        if(obj.sid==id) this.anewLinkObj = Object.assign({}, this.anewLinkObj, obj);
      }
    },
    closeAnew() {
      this.$emit("an-close-anew");
    },
    checkAnew(e) {
      // 新規保存
      console.log(e);
    }
  },
}
</script>

<style scoped>
@import "../css/anew.css";
</style>