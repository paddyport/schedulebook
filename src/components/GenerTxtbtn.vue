<template>
  <button type="button"
    :class="['btnText', btnCls, btnFlg ? '' : 'isNoActive']"
    @click="onClick">
    <span v-if="iconFlg"><i></i>{{ btnStr }}</span>
    <span v-else><span>{{ btnStr }}</span></span>
  </button>
</template>

<script>
export default {
// SU Component
  name: 'GenerTxtbtn',
  props: {
    iconFlg: Boolean,
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