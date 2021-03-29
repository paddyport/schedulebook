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
    mmbArr: Array,
    lblArr: Array,
  },
	data() {
		return {
      tipFlg: false,
    }
  },
  components: {
    GenerHead,
    GenerCclbtn,
  },
  methods: {
    closeChart() {
      this.$emit("ap-close-chart");
    },
  },
}
</script>

<style scoped>
@import "../css/chart.css";
</style>