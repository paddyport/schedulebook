<template>
  <div class="header">
    <common-icnbtn
      :btnFlg="true"
      :btnStr="'メニュー'"
      :btnCls="'def nml cng'"
      @icnbtn-click="oclick">
    </common-icnbtn>
    <common-head
      :headStr="currentYYMM.yy+'年'+currentYYMM.mm+'月'">
    </common-head>
  </div>
</template>

<script>
import CommonIcnbtn from '../parts/CommonIcnbtn'
import CommonHead from '../parts/CommonHead'

export default {
  name: 'CalendarHeader',
  props: {
    currentYYMM: Object,
    changeMonthBtnFlg: Boolean,
  },
  components: {
    CommonIcnbtn,
    CommonHead,
  },
  methods: {
    oclick() {

    }
  },
}
</script>