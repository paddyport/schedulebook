<template>
<div class="lbllist">
  <ul v-if="lblArr.length">
    <li
      v-for="(sl, slidx) in sortLblArr"
      :class="[sl.lid==checkLidObj.lid ? 'isChecked' : '']"
      :key="slidx">
      <a
        :data-sid="sl.lid"
        @click="checkLbl"
      >{{ sl.title }}</a>
    </li>
  </ul>
  <p
    v-else
    class="note"
  >ラベルがありません。</p>
</div>
</template>

<script>
export default {
// SU Component
  name: 'GenerLbllist',
  props: {
    checkLid: Number,
    lblArr: Array,
  },
  data() {
    return {
      checkLidObj: {sid: this.checkLid},
      sortLblArr: this.lblArr,
    }
  },
  created: function(){
    // this.sortTimetoArr();
    if(this.checkLid) this.getLidObj(this.checkLid);
  },
  methods: {
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
    // checkLbl(e) {
    //   const btn = e.target,
		// 		id = Number(btn.dataset.sid);
    //   this.getSidObj(id);
    //   this.$emit("change-link", id);
    // },
  },
}
</script>

<style scoped>
@import "../css/lbllist.css";
</style>