<template>
  <textarea
    :value="textareaValue ? textareaValue : ''"
    @blur="onBlur">
  </textarea>
</template>

<script>
export default {
  name: 'GenerTxtarea',
  props: {
    textareaValue: String,
  },
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