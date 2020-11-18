<template>
<div class="scdlist">
  <ul v-if="scdArr.length">
    <li
      v-for="(sa, saidx) in scdArr"
      :class="[sa.sid==checkSidObj.sid ? 'isChecked' : '']"
      :key="saidx">
      <a
        :data-sid="sa.sid"
        @click="checkScd"
      >{{ sa.head }}</a>
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
  },
  data() {
    return {
      checkSidObj: {sid: this.checkSid},
    }
  },
  created: function(){
    if(this.checkSid) this.getSidObj(this.checkSid);
  },
  methods: {
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