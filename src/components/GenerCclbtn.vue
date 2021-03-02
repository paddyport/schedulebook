<template>
  <button type="button" :class="['ccl', btnCls, btnFlg ? '' : 'isNoActive']" @click="onClick"></button>
</template>

<script>
export default {
  name: 'GenerCclbtn',
  props: {
    btnFlg: Boolean,
    btnCls: String,
  },
  methods: {
      onClick(e) {
          this.$emit("cclbtn-click", e);
      },
  },
}
</script>