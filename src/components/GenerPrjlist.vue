<template>
<div class="prjlist">
  <ul v-if="prjArr.length">
    <li
      v-for="(ss, ssidx) in sortPrjArr"
      :class="[ss.pid==checkPidObj.pid ? 'isChecked' : '']"
      :key="ssidx">
      <a
        :data-pid="ss.pid"
        @click="checkPrj"
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
  name: 'GenerPrjlist',
  props: {
    checkPid: Number,
    prjArr: Array,
    nowTime: Number,
  },
  data() {
    return {
      checkPidObj: {pid: this.checkPid},
      sortPrjArr: [],
    }
  },
  created: function(){
    this.sortTimetoArr();
    if(this.checkPid) this.getPidObj(this.checkPid);
  },
  methods: {
    sortTimetoArr() {
      for(let obj of this.prjArr) {
      console.log(this.nowTime, obj.end);
        if(this.nowTime<=obj.end) this.sortPrjArr.push(obj);
      }
      console.log(this.sortPrjArr);
    },
    getPidObj(id) {
      for(let obj of this.prjArr) {
        if(obj.pid==id) this.checkPidObj = Object.assign({}, this.checkPidObj, obj);
      }
    },
    checkPrj(e) {
      const btn = e.target,
				id = Number(btn.dataset.pid);
      this.getPidObj(id);
      this.$emit("change-link", id);
    },
  },
}
</script>

<style scoped>
@import "../css/prjlist.css";
</style>