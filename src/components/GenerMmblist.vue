<template>
<div class="mmblist">
  <div>
    <input
      type="text"
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
      :class="[choiceMmbArr&&choiceMmbArr.some(item=>item.mid==sm.mid) ? 'isChecked' : candidateMmbName&&sm.name.indexOf(candidateMmbName)>-1 ?'isCandidate' : '']"
      :key="smidx">
      <a
        :data-mid="sm.mid"
        @click="choiceMmb">
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
    choiceMmbArr: Array,
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
    // if(this.checkMid) this.getMidObj(this.checkMid);
  },
  methods: {
    // sortTimetoArr() {
    //   for(let obj of this.prjArr) {
    //   console.log(this.nowTime, obj.end);
    //     if(this.nowTime<=obj.end) this.sortPrjArr.push(obj);
    //   }
    //   console.log(this.sortPrjArr);
    // },
    // getMidObj(id) {
    //   for(let obj of this.mmbArr) {
    //     if(obj.mid==id) this.checkMidObj = Object.assign({}, this.checkMidObj, obj);
    //   }
    // },
    addMmbName(e) {
      const val = e.target.value;
      if(!val) return;
      this.addMmbFlg = true;
      this.candidateMmbName = val;
    },
    addMmb(e) {
        console.log("a");
        console.log(e.target.previousElementSibling);
    },
    choiceMmb(e) {
      const btn = e.target,
				id = Number(btn.dataset.mid);
      if(this.choiceMmbArr.some(item=>item.mid==id)) {
        this.$emit("rem-member", id);
      } else {
        // this.getMidObj(id);
        this.$emit("add-member", id);
      }
    },
  },
}
</script>

<style scoped>
@import "../css/mmblist.css";
</style>