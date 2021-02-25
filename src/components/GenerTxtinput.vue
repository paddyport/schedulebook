<template>
  <input 
    type="text"
    :placeholder="inputPlaceholder"
    @blur="onBlur">
</template>

<script>
export default {
// SU Component
  name: 'GenerTxtarea',
  props: {
    inputPlaceholder: String,
  },
  methods: {
    onBlur(e) {
      const str = e.target.value;
      if(!str || !str.match(/\S/g)) {
        e.target.value = "";
        return;
      }
      this.$emit("txtinput-blur", str);
    },
  },
}
</script>