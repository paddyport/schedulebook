<template>
  <div id="AnewScd" class="anew">
    <div>
      <div class="title">
        <input type="text" placeholder="新規タイトル">
      </div>
      <div class="date">
        <GenerWrdbtn
          v-if="!dateFlg&&!selDateFlg"
          :icon-flg="!dateFlg"
          :btn-flg="true"
          :btn-str="'選択'"
          :btn-cls="'def nml dtp'"
          @wrdbtn-click="switchDatepicker">
        </GenerWrdbtn>
        <GenerIcnbtn
          v-else
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
      <div class="color">
        <GenerWrdbtn
          v-if="!colorFlg&&!anewColor"
          :icon-flg="!colorFlg&&!anewColor"
          :btn-flg="true"
          :btn-str="'選択'"
          :btn-cls="'def nml cpr'"
          @wrdbtn-click="switchColopicker">
        </GenerWrdbtn>
        <GenerIcnbtn
          v-else
          :btn-flg="true"
          :btn-str="'選択'"
          :btn-cls="'def nml cpr'"
          @icnbtn-click="switchColopicker">
        </GenerIcnbtn>
        <p class="res"><i v-show="anewColor" :style="{background: anewColor}"></i>{{ anewColor }}</p>
        <GenerColorpicker
          v-if="colorFlg"
          :barIdx="anewPaletteIdx"
          @change-swatch="changeAnewColor">
        </GenerColorpicker>
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
        :btn-cls="'def nml scd'"
        @txtbtn-click="checkAnew">
      </GenerTxtbtn>
    </div>
  </div>
</template>

<script>
import GenerDatepicker from './GenerDatepicker'
import GenerColorpicker from './GenerColorpicker'
import GenerIcnbtn from './GenerIcnbtn'
import GenerWrdbtn from './GenerWrdbtn'
import GenerTxtbtn from './GenerTxtbtn'

export default {
  name: 'AnewLayerScd',
  props: {
    nowYear: Number,
    nowMonth: Number,
    nowDate: Number,
    ctgName: String,
    markYear: Number,
    markMonth: Number,
    markDate: Number,
  },
	data() {
		return {
      colorFlg: false,
      anewColor: "",
      anewPaletteIdx: 0,
      dateFlg: false,
      selDateFlg: false,
      anewStartTime: new Date(this.markYear, this.markMonth, this.markDate).getTime(),
      selStartYyMmDd: {yy: this.markYear, mm: this.markMonth, dd: this.markDate},
      anewEndTime: new Date(this.markYear, this.markMonth, this.markDate).getTime(),
      selEndYyMmDd: {yy: this.markYear, mm: this.markMonth, dd: this.markDate},
		}
  },
  components: {
    GenerDatepicker,
    GenerColorpicker,
    GenerIcnbtn,
    GenerWrdbtn,
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
      this.selDateFlg = this.dateFlg ? true : this.selDateFlg;
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
      this.selDateFlg = true;
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