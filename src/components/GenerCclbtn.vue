<template>
  <button type="button" :class="['btnCircle', btnCls, btnFlg ? '' : 'isNoActive']" @click="onClick">
    <span>{{ btnStr }}</span>
  </button>
</template>

<script>
export default {
  name: 'GenerCclbtn',
  props: {
    btnFlg: Boolean,
    btnStr: String,
    btnCls: String,
  },
  methods: {
      onClick(e) {
          this.$emit("cclbtn-click", e);
      },
  },
}
</script>