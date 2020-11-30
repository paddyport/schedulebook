<template>
<div class="lbllist">
  <div>
    <i 
      :style="{background: addLabelColor}"
      @click="switchColopicker"></i>
    <GenerColorpicker
      v-if="colorFlg"
      :bar-idx="anewPaletteIdx"
      :sel-color="anewColor"
      @change-swatch="addLblColor">
    </GenerColorpicker>
    <input
      type="text"
      @focus="inputLblTitle"
      @blur="addLblTitle">
    <GenerWrdbtn
      :icon-flg="false"
      :btn-flg="addLabelFlg"
      :btn-str="'追加'"
      :btn-cls="'def nml widSM'"
      @wrdbtn-click="addLbl">
    </GenerWrdbtn>
  </div>
  <ul v-if="lblArr.length">
    <li
      v-for="(sl, slidx) in sortLblArr"
      :class="[sl.lid==checkLidObj.lid ? 'isChecked' : '']"
      :key="slidx">
      <a
        :data-lid="sl.lid"
        @click="changeLbl"
      >
        <i :style="{background: sl.color}"></i>
        {{ sl.title }}</a>
    </li>
  </ul>
  <p
    v-else
    class="note"
  >ラベルがありません。</p>
</div>
</template>

<script>
import GenerColorpicker from './GenerColorpicker'
import GenerWrdbtn from './GenerWrdbtn'

export default {
// SU Component
  name: 'GenerLbllist',
  props: {
    checkLid: Number,
    lblArr: Array,
    // barIdx: Number,
    // selColor: String,
  },
  data() {
    return {
      checkLidObj: {sid: this.checkLid},
      sortLblArr: this.lblArr,
      colorFlg: false,
      addLabelFlg: false,
      anewColor: "#fee7ed",
      anewPaletteIdx: 0,
      addLabelColor: "",
      addLabelTitle: "",
    }
  },
  components: {
    GenerColorpicker,
    GenerWrdbtn,
  },
  created: function(){
    // this.sortTimetoArr();
    if(this.checkLid) this.getLidObj(this.checkLid);
  },
  methods: {
    switchColopicker() {
      this.colorFlg = this.colorFlg ? false : true;
    },
    // sortTimetoArr() {
    //   for(let obj of this.scdArr) {
    //   console.log(this.nowTime, obj.end);
    //     if(this.nowTime<=obj.end) this.sortScdArr.push(obj);
    //   }
    //   console.log(this.sortScdArr);
    // },
    getLidObj(id) {
      for(let obj of this.lblArr) {
        if(obj.lid==id) this.checkLidObj = Object.assign({}, this.checkLidObj, obj);
      }
    },
    addLblColor(val, idx) {
      this.anewColor = val;
      this.anewPaletteIdx = idx;
      this.addLabelColor = val;
    },
    inputLblTitle() {
      this.colorFlg = false;
    },
    addLblTitle(e) {
      const val = e.target.value;
      if(val) {
        this.addLabelFlg = true;
        this.addLabelTitle = val;
      }
    },
    addLbl() {
      this.colorFlg = false;
    },
    changeLbl(e) {
      const btn = e.target,
				id = Number(btn.dataset.lid);
      this.getLidObj(id);
      this.$emit("change-label", id);
    },
  },
}
</script>

<style scoped>
@import "../css/lbllist.css";
</style>