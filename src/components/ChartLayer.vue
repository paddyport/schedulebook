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
      <div
        :style="{width: chartWidth+'px'}">
        <div
          v-for="n in chartNum"
          class="day"
          :key="n">
          <div class="month"
            :style="{width:  n==1 || new Date(prjData.start+(n-1)*dayTime).getDate()==1 ?chartMonthWidth[monthName[new Date(prjData.start+(n-1)*dayTime).getMonth()]]+'px' : ''}">
            <span
              v-if="n==1 || new Date(prjData.start+(n-1)*dayTime).getDate()==1">
              {{ new Date(prjData.start+(n-1)*dayTime).getMonth()+1 }}月
            </span>
          </div>
          <div class="date">
            <span>{{ new Date(prjData.start+(n-1)*dayTime).getDate() }}</span>
          </div>
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
      dayWidth: document.body.clientWidth/10,
      chartNum: 0,
      chartWidth: 0,
      monthName: ["jan", "feb", "mar", "apr", "may", "jun", "jul", "aug", "sep", "oct", "nov", "dec"],
      chartMonthWidth: {"jan": 0, "feb": 0, "mar": 0, "apr": 0, "may": 0, "jun": 0, "jul": 0, "aug": 0, "sep": 0, "oct": 0, "nov": 0, "dec": 0},
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
      this.chartNum = this.chartNum<10 ? 10 : this.chartNum;
      this.chartWidth = this.chartNum*this.dayWidth;
      for(let d=0;d<this.chartNum;d++) {
        const m = this.monthName[new Date(this.prjData.start+d*this.dayTime).getMonth()], 
          nn = this.chartMonthWidth[m]+this.dayWidth,
          obj = this.chartMonthWidth;
        obj[m] = nn;
        this.chartMonthWidth = Object.assign(this.chartMonthWidth, obj);
      }
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