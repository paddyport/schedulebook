<template>
  <button type="button" :class="['icn', btnCls, btnFlg ? '' : 'isNoActive']" @click="onClick">
  </button>
</template>

<script>
export default {
// SU Component
  name: 'GenerIcnbtn',
  props: {
    btnFlg: Boolean,
    btnCls: String,
  },
  methods: {
      onClick(e) {
          this.$emit("icnbtn-click", e);
      },
  },
}
</script>