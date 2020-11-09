<template>
  <button type="button" :class="['btnIcon', btnCls, btnFlg ? '' : 'isNoActive']" @click="onClick">
    <span v-if="btnStr"><i></i>{{ btnStr }}</span>
    <span v-else><i></i></span>
  </button>
</template>

<script>
export default {
// SU Component
  name: 'GenerIcnbtn',
  props: {
    btnFlg: Boolean,
    btnStr: String,
    btnCls: String,
  },
  methods: {
      onClick(e) {
          this.$emit("icnbtn-click", e);
      },
  },
}
</script>