<template>
  <input 
    type="text"
    :placeholder="!inputValue ? inputPlaceholder : ''"
    :value="inputValue ? inputValue : ''"
    @blur="onBlur">
</template>

<script>
export default {
  name: 'GenerTxtarea',
  props: {
    inputPlaceholder: String,
    inputValue: String,
  },
  methods: {
    onBlur(e) {
      const str = e.target.value;
      if(this.inputValue && str==this.inputValue) return;
      if(!str || !str.match(/\S/g)) {
        e.target.value = "";
        return;
      }
      this.$emit("txtinput-blur", str);
    },
  },
}
</script>