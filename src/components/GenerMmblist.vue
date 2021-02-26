<template>
<div class="mmblist">
  <div>
    <input
      type="text"
      @keyup="checkInput"
      @blur="checkInput">
    <GenerWrdbtn
      :icon-flg="false"
      :btn-flg="addMmbFlg"
      :btn-str="'絞込'"
      :btn-cls="'def nml wrd'"
      @wrdbtn-click="sortMmbName">
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
  name: 'GenerMmblist',
  props: {
    mmbArr: Array,
    choiceMmbArr: Array,
  },
  data() {
    return {
      sortMmbArr: this.mmbArr,
      addMmbFlg: false,
      candidateMmbName: "",
    }
  },
  components: {
    GenerWrdbtn,
  },
  methods: {
    checkInput(e) {
      const val = e.target.value;
      if(!val) return;
      this.addMmbFlg = true;
    },
    sortMmbName(e) {
      const val = e.target.previousElementSibling.value;
      this.candidateMmbName = val;
    },
    choiceMmb(e) {
      const btn = e.target,
				id = Number(btn.dataset.mid);
      if(this.choiceMmbArr.some(item=>item.mid==id)) {
        this.$emit("rem-member", id);
      } else {
        this.$emit("add-member", id);
      }
    },
  },
}
</script>

<style scoped>
@import "../css/mmblist.css";
</style>