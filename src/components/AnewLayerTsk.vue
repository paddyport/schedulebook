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
          :btn-cls="'def nml lbl'"
          @icnbtn-click="switchLbllist">
        </GenerIcnbtn>
        <p class="res">
          <i v-show="anewLabelObj.color" :style="{background: anewLabelObj.color}"></i>
          {{ anewLabelObj.lid ? anewLabelObj.title : "－－" }}</p>
        <GenerLbllist
          v-if="labelFlg"
          :check-lid="0"
          :lbl-arr="lblArr"
          @change-label="changeAnewLabel"
          @change-swatch="changeAnewColor">
        </GenerLbllist>
      </div>
      <div class="link">
        <GenerIcnbtn
          :btn-flg="true"
          :btn-str="'選択'"
          :btn-cls="'def nml lnk'"
          @icnbtn-click="switchPrjlist">
        </GenerIcnbtn>
        <p class="res">{{ anewLinkObj.pid ? anewLinkObj.title : "－－" }}</p>
        <GenerPrjlist
          v-if="linkFlg"
          :check-pid="0"
          :prj-arr="prjArr"
          :now-time="new Date(this.nowYear, this.nowMonth, this.nowDate).getTime()"
          @change-link="changeAnewLink">
        </GenerPrjlist>
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
import GenerLbllist from './GenerLbllist'
import GenerPrjlist from './GenerPrjlist'
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
    lblArr: Array,
    prjArr: Array,
  },
	data() {
		return {
      dateFlg: false,
      anewStartTime: new Date(this.markYear, this.markMonth, this.markDate).getTime(),
      selStartYyMmDd: {yy: this.markYear, mm: this.markMonth, dd: this.markDate},
      anewEndTime: new Date(this.markYear, this.markMonth, this.markDate).getTime(),
      selEndYyMmDd: {yy: this.markYear, mm: this.markMonth, dd: this.markDate},
      labelFlg: false,
      addLblFlg: false,
      anewColor: "#fee7ed",
      anewPaletteIdx: 0,
      anewLabelObj: {lid: 0},
      linkFlg: false,
      anewLinkObj: {pid: 0},
		}
  },
  components: {
    GenerDatepicker,
    GenerLbllist,
    GenerPrjlist,
    GenerIcnbtn,
    // GenerWrdbtn,
    GenerTxtbtn,
  },
  methods: {
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
    switchLbllist() {
      this.labelFlg = this.labelFlg ? false : true;
      console.log(this.labelFlg);
    },
    changeAnewColor(val, idx) {
      this.anewColor = val;
      this.anewPaletteIdx = idx;
      this.addLblFlg = true;
    },
    changeAnewLabel(id) {
      console.log(id);
      for(let obj of this.lblArr) {
        if(obj.lid==id) {
          this.addLblFlg = false;
          this.anewLabelObj = Object.assign({}, this.anewLabelObj, obj);
        }
      }
    },
    switchPrjlist() {
      this.linkFlg = this.linkFlg ? false : true;
    },
    changeAnewLink(id) {
      for(let obj of this.prjArr) {
        if(obj.pid==id) this.anewLinkObj = Object.assign({}, this.anewLinkObj, obj);
      }
    },
    closeAnew() {
      this.$emit("ap-close-anew");
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