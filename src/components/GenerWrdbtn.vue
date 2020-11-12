<template>
  <button type="button"
    :class="['btnWord', btnCls, btnFlg ? '' : 'isNoActive']"
    @click="onClick">
    <span v-if="iconFlg"><i></i>{{ btnStr }}</span>
    <span v-else><span>{{ btnStr }}</span></span>
  </button>
</template>

<script>
export default {
// SU Component
  name: 'GenerWrdbtn',
  props: {
    iconFlg: Boolean,
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