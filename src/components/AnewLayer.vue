<template>
  <div id="Anew" class="anew">
    <div>
      <div class="title">
        <input type="text" placeholder="新規タイトル">
      </div>
      <div class="date">
        <GenerWrdbtn
          v-if="!dateFlg"
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
        <div v-if="dateFlg" class="start">
          <a
            :class="['btn', startFlg ? 'isActive' : '']"
            @click="switchStartDatepicker">
            <span>開始</span>
          </a>
          <p class="res">{{ selStartYyMmDd.yy }}年{{ selStartYyMmDd.mm+1 }}月{{ selStartYyMmDd.dd }}日</p>
          <GenerDatepicker
            v-if="startFlg"
            :now-yy-mm-dd="{yy: this.nowYear, mm: this.nowMonth, dd: this.nowDate}"
            :mark-yy-mm-dd="selStartYyMmDd"
            @select-date="selectAnewStart">
          </GenerDatepicker>
        </div>
        <div v-if="dateFlg" class="end">
          <a
            :class="['btn', anewStartTime ? '' : 'isNoActive', endFlg ? 'isActive' : '']"
            @click="switchEndDatepicker">
            <span>終了</span>
          </a>
          <p
            v-if="anewStartTime"
            class="res">{{ selEndYyMmDd.yy }}年{{ selEndYyMmDd.mm+1 }}月{{ selEndYyMmDd.dd }}日</p>
          <GenerDatepicker
            v-if="endFlg"
            :now-yy-mm-dd="{yy: this.nowYear, mm: this.nowMonth, dd: this.nowDate}"
            :mark-yy-mm-dd="selEndYyMmDd"
            @select-date="selectAnewEnd">
          </GenerDatepicker>
        </div>
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
        :btn-cls="'def nml '+ctgName"
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
  name: 'AnewLayer',
  props: {
    now: Object,
    nowYear: Number,
    nowMonth: Number,
    nowDate: Number,
    ctgName: String,
    markYyMmDd: Object,
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
      selStartYyMmDd: {yy: this.markYear, mm: this.markMonth, dd: this.markDate},
      startFlg: false,
      anewStartTime: 0,
      selEndYyMmDd: {yy: this.markYear, mm: this.markMonth, dd: this.markDate},
      endFlg: false,
      anewEndTime: 0,
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
    },
    switchStartDatepicker() {
      this.startFlg = this.startFlg ? false : true;
    },
    selectAnewStart(val) {
      this.anewStartTime = val;
    },
    switchEndDatepicker() {
      this.endFlg = this.endFlg ? false : true;
    },
    selectAnewEnd(val) {
      this.anewEndTime = val;
    },
    closeAnew() {
      this.$emit("an-close-anew");
    },
    checkAnew(e) {
      console.log(e);
    }
  },
}
</script>

<style scoped>
@import "../css/anew.css";
</style>