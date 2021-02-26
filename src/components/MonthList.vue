<template>
  <div id="MonthList" class="monthlist">
    <ul>
      <li v-for="(ml, mlidx) in monthListArr" class="day" :key="mlidx">
        <a></a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'MonthList',
  props: {
    nowYyMm: Object,
  },
  data() {
    return {
      monthListArr: [],
    }
  },
  created: function(){
    this.setList();
  },
  methods: {
    setList() {

    },
  },
}
</script>