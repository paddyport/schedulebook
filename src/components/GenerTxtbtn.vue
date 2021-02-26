<template>
  <button type="button"
    :class="['txt', btnCls, btnFlg ? '' : 'isNoActive']"
    @click="onClick">
    {{ btnStr }}
  </button>
</template>

<script>
export default {
  name: 'GenerTxtbtn',
  props: {
    btnFlg: Boolean,
    btnStr: String,
    btnCls: String,
  },
  methods: {
    onClick(e) {
      this.$emit("txtbtn-click", e);
    },
  },
}
</script>