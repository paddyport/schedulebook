<template>
<div class="datepicker">
  <div class="heads">
    <button
      type="button"
      :class="['prv', datingYyMm.year==(nowYyMm.year-1)&&datingYyMm.month==(nowYyMm.month+1) ? 'isNoActive' : '']"
      @click="shiftMonth">
    </button>
    <p>{{ datingYyMm.year }}年{{ datingYyMm.month+1 }}月</p>
    <button
      type="button"
      :class="['nxt', datingYyMm.year==(nowYyMm.year+1)&&datingYyMm.month==(nowYyMm.month-1) ? 'isNoActive' : '']"
      @click="shiftMonth">
    </button>
  </div>
  <div class="dates">
    <div v-for="(da, daidx) in datingArr" class="month" v-bind:key="daidx">
      <a
        v-if="da"
        :class="[startTime<da&&da<endTime ? 'isBet' : '', startTime==da||endTime==da ? 'isFixed' : '', startTime==da ? 'isStart' : '', endTime==da ? 'isEnd' : '']"
        @click="selectDating"
      >{{ da.getDate() }}</a>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: "GenerDatepicker",
  props: {
    nowYyMmDd: Object,
    startTime: Number,
    endTime: Number,
  },
  data() {
    return {
        datingYyMmDd: {},
        datingArr: [],
    }
  },
  methods: {
    getDating(yymmdd) {
      this.datingYyMmDd = yymmdd ? yymmdd : this.datingYyMm;
    },
    shiftMonth() {

    },
    selectDating(e) {
      console.log(e);
      this.$emit("select-date");
    },
  },
}
</script>

<style scoped>
@import "../css/colorpicker.css";
</style>