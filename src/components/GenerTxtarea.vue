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
      const str = e.target.value;
      if(!str || !str.match(/\S/g)) {
        e.target.value = "";
        return;
      }
      this.$emit("txtarea-blur", str);
    },
  },
}
</script>