<template>
<div class="mmblist">
  <div>
    <input
      type="text"
      @focus="inputMmbName"
      @blur="addMmbName">
    <GenerWrdbtn
      :icon-flg="false"
      :btn-flg="addMmbFlg"
      :btn-str="'追加'"
      :btn-cls="'def nml wrd'"
      @wrdbtn-click="addMmb">
    </GenerWrdbtn>
  </div>
  <ul v-if="mmbArr.length">
    <li
      v-for="(sm, smidx) in sortMmbArr"
      :class="[sm.mid==checkMidObj.mid ? 'isChecked' : sm.name==candidateMmbName ?'isCandidate' : '']"
      :key="smidx">
      <a
        :data-mid="sm.mid"
        @click="choiceMmb"
      >
        <figure><img :src="sm.icon" :alt="sm.name"></figure>
        <p>{{ sm.name }}</p>
      </a>
    </li>
  </ul>
</div>
</template>

<script>
import GenerWrdbtn from './GenerWrdbtn'

export default {
// SU Component
  name: 'GenerMmblist',
  props: {
    checkMid: Number,
    mmbArr: Array,
  },
  data() {
    return {
      checkMidObj: {pid: this.checkMid},
      sortMmbArr: this.mmbArr,
      addMmbFlg: false,
      candidateMmbName: "",
    }
  },
  components: {
    GenerWrdbtn,
  },
  created: function(){
    // this.sortTimetoArr();
    if(this.checkMid) this.getMidObj(this.checkMid);
  },
  methods: {
    // sortTimetoArr() {
    //   for(let obj of this.prjArr) {
    //   console.log(this.nowTime, obj.end);
    //     if(this.nowTime<=obj.end) this.sortPrjArr.push(obj);
    //   }
    //   console.log(this.sortPrjArr);
    // },
    getMidObj(id) {
      for(let obj of this.mmbArr) {
        if(obj.mid==id) this.checkMidObj = Object.assign({}, this.checkMidObj, obj);
      }
    },
    addMmbName(e) {
      const val = e.target.value;
      if(val) {
        this.candidateMmbName = val;
      }
    },
    addMmb() {
        console.log("a");
    },
    choiceMmb(e) {
      const btn = e.target,
				id = Number(btn.dataset.mid);
      this.getMidObj(id);
      this.$emit("choice-member", id);
    },
  },
}
</script>

<style scoped>
@import "../css/mmblist.css";
</style>