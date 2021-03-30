<template>
  <div id="Chart" class="chart">
    <div class="header">
      <GenerHead
        :head-str="prjData.title">
      </GenerHead>
      <GenerCclbtn
        :btn-flg="true"
        :btn-cls="'icn nml cls'"
        @click="closeChart">
      </GenerCclbtn>
    </div>
    <div class="body">
      <div>
        <div
          v-for="n in chartNum"
          class="day"
          :key="n">
          <div
            v-for="(ta, taidx) in tskArr"
            class="task"
            :key="taidx">
            <div
              v-if="ta.start<=(prjData.start+(n-1)*dayTime)&&(prjData.start+(n-1)*dayTime)<=ta.end"
              :class="'stt'+ta.status"
              :data-tid="ta.tid"
              @click="openTooltip">
              <span v-if="(prjData.start+(n-1)*dayTime)==ta.start">
                {{ ta.title }}
              </span>
            </div>
          </div>
          <div
            v-for="s in (9-tskArr.length)"
            class="task"
            :key="s">
          </div>
        </div>
        <div
          v-if="tipFlg"
          class="ttip">
          <h2>{{ tipTsk.title }}</h2>
          {{ tipTsk.member }}
          {{ tipTsk.lid }}
          {{ tipTsk.status }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import GenerHead from './GenerHead'
import GenerCclbtn from './GenerCclbtn'

export default {
  name: 'ChartLayer',
  props: {
    prjData: Object,
    tskArr: Array,
    mmbArr: Array,
    lblArr: Array,
  },
	data() {
		return {
      dayTime: 1000*60*60*24,
      chartNum: 0,
      chartTskArr: [],
      tipFlg: false,
      tipTsk: {},
    }
  },
  components: {
    GenerHead,
    GenerCclbtn,
  },
  created: function(){
    this.setChartArr();
  },
  methods: {
    setChartArr() {
      this.chartNum = (this.prjData.end-this.prjData.start)/this.dayTime+1;
      this.chartNum = this.chartNum<11 ? 11 : this.chartNum;
    },
    openTooltip(e) {
      const tid = e.target.dataset.tid;
      this.tipTsk = this.tskArr[this.tskArr.findIndex(function(item){return item.tid==tid})];
      this.tipFlg = true;
      console.log(this.tipTsk);
    },
    closeChart() {
      this.$emit("ap-close-chart");
    },
  },
}
</script>

<style scoped>
@import "../css/chart.css";
</style>