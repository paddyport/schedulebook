<template>
  <div id="AnewPrj" class="anew">
    <div>
      <div class="title">
        <GenerTxtinput
          :input-placeholder="'新規タイトル'"
          @txtinput-blur="checkText">
        </GenerTxtinput>
      </div>
      <div class="date">
        <GenerIcnbtn
          :btn-flg="true"
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
      <div class="member">
        <GenerIcnbtn
          :btn-flg="true"
          :btn-cls="'def nml mmb'"
          @icnbtn-click="switchMmblist">
        </GenerIcnbtn>
        <div class="res">
          <ul v-if="choiceMmbArr.length">
            <li
              v-for="(cm, cmidx) in choiceMmbArr"
              :key="cmidx">
              <figure>
                <img :src="cm.icon" :alt="cm.name">
              </figure>
            </li>
          </ul>
        </div>
        <GenerMmblist
          v-if="memberFlg"
          :check-mid="checkMid"
          :mmb-arr="mmbArr"
          :choice-mmb-arr="choiceMmbArr"
          @rem-member="remMember"
          @add-member="addMember">
        </GenerMmblist>
      </div>
      <div class="caption">
        <GenerTxtarea></GenerTxtarea>
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
        :btn-flg="anewBtnFlg"
        :btn-str="'新規作成'"
        :btn-cls="'def nml prj'"
        @txtbtn-click="checkAnew">
      </GenerTxtbtn>
    </div>
  </div>
</template>

<script>
import GenerDatepicker from './GenerDatepicker'
import GenerMmblist from './GenerMmblist'
import GenerIcnbtn from './GenerIcnbtn'
import GenerTxtbtn from './GenerTxtbtn'
import GenerTxtarea from './GenerTxtarea'
import GenerTxtinput from './GenerTxtinput'

export default {
  name: 'AnewLayerPrj',
  props: {
    nowYear: Number,
    nowMonth: Number,
    nowDate: Number,
    ctgName: String,
    markYear: Number,
    markMonth: Number,
    markDate: Number,
    mmbArr: Array,
  },
	data() {
		return {
      anewTitle: "",
      dateFlg: false,
      anewStartTime: new Date(this.markYear, this.markMonth, this.markDate).getTime(),
      selStartYyMmDd: {yy: this.markYear, mm: this.markMonth, dd: this.markDate},
      anewEndTime: new Date(this.markYear, this.markMonth, this.markDate).getTime(),
      selEndYyMmDd: {yy: this.markYear, mm: this.markMonth, dd: this.markDate},
      memberFlg: false,
      choiceMmbArr: [],
      anewBtnFlg: false,
		}
  },
  components: {
    GenerDatepicker,
    GenerMmblist,
    GenerIcnbtn,
    GenerTxtbtn,
    GenerTxtinput,
    GenerTxtarea,
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
    switchMmblist() {
      this.memberFlg = this.memberFlg ? false : true;
    },
    remMember(id) {
      console.log(id, this.choiceMmbArr);
      this.choiceMmbArr = this.choiceMmbArr.filter(function(arr){return arr.mid!==id});
      console.log(this.choiceMmbArr);
    },
    addMember(id) {
      const obj = this.mmbArr.filter(function(arr){return arr.mid===id});
      this.choiceMmbArr.push(obj[0]);
    },
    closeAnew() {
      this.$emit("an-close-anew");
    },
    checkText(str) {
      this.anewBtnFlg = true;
      this.anewTitle = str;
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