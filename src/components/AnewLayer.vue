<template>
  <div id="Anew" class="anew">
    <div>
      <div class="title">
        <input type="text" placeholder="新規タイトル">
      </div>
      <div class="date">
        <div class="start">
          <GenerWrdbtn
            v-if="!startFlg&&!anewStartTime"
            :icon-flg="!startFlg&&!anewStartTime"
            :btn-flg="true"
            :btn-str="'選択'"
            :btn-cls="'def nml dtp'"
            @wrdbtn-click="switchStartDatepicker">
          </GenerWrdbtn>
          <GenerIcnbtn
            v-else
            :btn-flg="true"
            :btn-str="'選択'"
            :btn-cls="'def nml dtp'"
            @icnbtn-click="switchStartDatepicker">
          </GenerIcnbtn>
          <p class="code">{{ selStartYyMmDd.yy }}年{{ selStartYyMmDd.mm+1 }}月{{ selStartYyMmDd.dd }}日</p>
          <GenerDatepicker
            ref="datepicker"
            v-show="startFlg"
            @select-date="selectAnewStart">
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
        <p class="code"><i v-show="anewColor" :style="{background: anewColor}"></i>{{ anewColor }}</p>
        <GenerColorpicker
          ref="colorpicker"
          v-show="colorFlg"
          @change-swatch="changeAnewColor">
        </GenerColorpicker>
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
  </div>
</template>

<script>
// import GenerDatepicker from './GenerDatepicker'
import GenerColorpicker from './GenerColorpicker'
import GenerIcnbtn from './GenerIcnbtn'
import GenerWrdbtn from './GenerWrdbtn'
import GenerTxtbtn from './GenerTxtbtn'

export default {
  name: 'AnewLayer',
  props: {
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
      // selStartYyMmDd: {},
      selStartYyMmDd: {yy: this.markYear, mm: this.markMonth, dd: this.markDate},
      // selStartYyMmDd: this.markYyMmDd,
      // selEndYyMmDd: this.markYyMmDd,
      startFlg: false,
      anewStartTime: 0,
		}
  },
  components: {
    // GenerDatepicker,
    GenerColorpicker,
    GenerIcnbtn,
    GenerWrdbtn,
    GenerTxtbtn,
  },
  methods: {
    switchColopicker() {
      this.colorFlg = this.colorFlg ? false : true;
      if(this.colorFlg) this.$refs.colorpicker.getPaletteSwatch();
    },
    changeAnewColor(val) {
      // this.colorFlg = this.colorFlg ? false : true;
      this.anewColor = val;
    },
    switchStartDatepicker() {
      this.startFlg = this.startFlg ? false : true;
      // this.selStartYyMmDd = {yy: this.markYear, mm: this.markMonth, dd: this.markDate};
      // $set
      console.log(this.selStartYyMmDd);
      // if(this.startFlg) this.$refs.datepicker.getDating(this.selStartYyMmDd);
    },
    selectAnewStart(val) {
      this.anewStartTime = val;
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