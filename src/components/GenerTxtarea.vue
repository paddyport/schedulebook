<template>
  <textarea
    @blur="onBlur">
  </textarea>
</template>

<script>
export default {
// SU Component
  name: 'GenerTxtarea',
  methods: {
    onBlur(e) {
      this.$emit("txtarea-blur", e);
    },
  },
}
</script>