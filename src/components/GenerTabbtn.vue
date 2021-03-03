<template>
  <button type="button"
    :class="['tab', btnCls, currentFlg ? 'isCurrent' : '', btnFlg ? '' : 'isNoActive']"
    @click="onClick">
    {{ btnStr }}
  </button>
</template>

<script>
export default {
  name: 'GenerTabbtn',
  props: {
    currentFlg: Boolean,
    btnFlg: Boolean,
    btnStr: String,
    btnCls: String,
    btnSubmit: String,
  },
  methods: {
    onClick() {
      this.$emit("tabbtn-click", this.btnSubmit);
    },
  },
}
</script>