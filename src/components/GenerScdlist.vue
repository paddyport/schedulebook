<template>
<div>
</div>
</template>

<script>
export default {
// SU Component
  name: 'GenerScdlist',
  props: {
    db: Object,
    checkScd: Number,
  },
  data() {
    return {
      checkScdObj: {},
      scdArr: [],
    }
  },
  created: function(){
    this.getScdArr();
  },
  methods: {
      getScdArr() {
				that.db.tsk.toArray().then((list) => {
          for(let obj of list) {
            if(checkScd) this.$set(this.checkScdObj, obj);
            this.scdArr.push(obj);
          }
				});
      },
  },
}
</script>