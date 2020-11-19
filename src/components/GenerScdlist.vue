<template>
<div class="scdlist">
  <ul v-if="scdArr.length">
    <li
      v-for="(ss, ssidx) in sortScdArr"
      :class="[ss.sid==checkSidObj.sid ? 'isChecked' : '']"
      :key="ssidx">
      <a
        :data-sid="ss.sid"
        @click="checkScd"
      >{{ ss.title }}</a>
    </li>
  </ul>
  <p
    v-else
    class="note"
  >イベントがありません。</p>
</div>
</template>

<script>
export default {
// SU Component
  name: 'GenerScdlist',
  props: {
    checkSid: Number,
    scdArr: Array,
    nowTime: Number,
  },
  data() {
    return {
      checkSidObj: {sid: this.checkSid},
      sortScdArr: [],
    }
  },
  created: function(){
    this.sortTimetoArr();
    if(this.checkSid) this.getSidObj(this.checkSid);
  },
  methods: {
    sortTimetoArr() {
      for(let obj of this.scdArr) {
      console.log(this.nowTime, obj.end);
        if(this.nowTime<=obj.end) this.sortScdArr.push(obj);
      }
      console.log(this.sortScdArr);
    },
    getSidObj(id) {
      for(let obj of this.scdArr) {
        if(obj.sid==id) this.checkSidObj = Object.assign({}, this.checkSidObj, obj);
      }
    },
    checkScd(e) {
      const btn = e.target,
				id = Number(btn.dataset.sid);
      this.getSidObj(id);
      this.$emit("change-link", id);
    },
  },
}
</script>

<style scoped>
@import "../css/scdlist.css";
</style>