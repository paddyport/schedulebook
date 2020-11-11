<template>
  <div id="Anew" class="anew">
    <div>
      <div class="title">
        <input type="text" placeholder="新規タイトル">
      </div>
      <div class="date">

      </div>
      <div class="color">
        <GenerTxtbtn
          v-if="!colorFlg&&!anewColor"
          :icon-flg="!colorFlg&&!anewColor"
          :btn-flg="true"
          :btn-str="'選択'"
          :btn-cls="anewColorBtnCls"
          @txtbtn-click="switchColopicker">
        </GenerTxtbtn>
        <GenerIcnbtn
          v-else
          :btn-flg="true"
          :btn-str="'選択'"
          :btn-cls="'def nml cpr'"
          @icnbtn-click="switchColopicker">
        </GenerIcnbtn>
        <p class="code"><i v-show="anewColor" :style="{background: anewColor}"></i>{{ anewColor }}</p>
        <GenerColorpicker
          ref="picker"
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
import GenerColorpicker from './GenerColorpicker'
import GenerIcnbtn from './GenerIcnbtn'
import GenerTxtbtn from './GenerTxtbtn'

export default {
  name: 'AnewLayer',
  props: {
    ctgName: String,
    markYyMm: Object,
  },
	data() {
		return {
      colorFlg: false,
      anewColor: "",
      anewColorBtnCls: "def nml cpr",
      anewPallete: "",
		}
  },
  components: {
    GenerColorpicker,
    GenerIcnbtn,
    GenerTxtbtn,
  },
  methods: {
    switchColopicker() {
      this.colorFlg = this.colorFlg ? false : true;
      if(this.colorFlg) this.$refs.picker.getPaletteSwatch(this.anewPallete);
    },
    changeAnewColor(val) {
      // this.colorFlg = this.colorFlg ? false : true;
      this.anewColor = val;
      this.anewColorBtnCls = "def cpr";
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