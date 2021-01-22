<template>
  <button type="button"
    :class="['wrd', btnCls, btnFlg ? '' : 'isNoActive']"
    @click="onClick">
    {{ btnStr }}
  </button>
</template>

<script>
export default {
// SU Component
  name: 'GenerWrdbtn',
  props: {
    btnFlg: Boolean,
    btnStr: String,
    btnCls: String,
  },
  methods: {
      onClick(e) {
          this.$emit("wrdbtn-click", e);
      },
  },
}
</script>