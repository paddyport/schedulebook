<template>
  <div id="ShowDate" class="show">
    <div class="header">
      <GenerHead
        :head-str="markYear+'年'+(markMonth+1)+'月'+markDate+'日'">
      </GenerHead>
      <GenerCclbtn
        :btn-flg="true"
        :btn-cls="'icn nml cls'"
        @click="closeAnew">
      </GenerCclbtn>
    </div>
    <div class="body">
      <ul v-if="showPrjArr.length">
      </ul>
      <ul v-if="showTskArr.length">
      </ul>
      <p v-if="!showPrjArr.length&&!showTskArr.length">何も登録されていません。</p>
    </div>
    <div class="footer">
      <GenerTxtbtn
        :btn-flg="true"
        :btn-str="'新規作成'"
        :btn-cls="'def nml prj'"
        @txtbtn-click="openAnewPrj">
      </GenerTxtbtn>
      <GenerTxtbtn
        :btn-flg="true"
        :btn-str="'新規作成'"
        :btn-cls="'def nml tsk'"
        @txtbtn-click="openAnewTsk">
      </GenerTxtbtn>
    </div>
  </div>
</template>

<script>
import GenerHead from './GenerHead'
// import GenerIcnbtn from './GenerIcnbtn'
import GenerCclbtn from './GenerCclbtn'
import GenerTxtbtn from './GenerTxtbtn'

export default {
  name: 'ShowLayerDate',
  props: {
    markYear: Number,
    markMonth: Number,
    markDate: Number,
    showPrjArr: Array,
    showTskArr: Array,
  },
	data() {
		return {
		}
  },
  components: {
    GenerHead,
    // GenerIcnbtn,
    GenerCclbtn,
    GenerTxtbtn,
  },
  methods: {
    openAnewPrj() {
      this.$emit("ap-shown-loader");
      this.$emit("ap-open-mark-anew-prj");
    },
    openAnewTsk() {
      this.$emit("ap-shown-loader");
      this.$emit("ap-open-mark-anew-tsk");
    },
    closeAnew() {
      this.$emit("ap-close-show");
    },
  },
}
</script>

<style scoped>
@import "../css/show.css";
</style>