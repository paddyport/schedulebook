<template>
  <div id="MonthList" class="monthlist">
    <ul>
      <li v-for="(ml, mlidx) in monthListArr" class="day" :key="mlidx">
        <a>
          {{ ml.yy }}年{{ ml.mm+1 }}月
          <span v-if="ml.prj.length ">{{ ml.prj.length }}</span>
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'MonthList',
  props: {
    monthListArr: Array,
  },
  methods: {
    onClick() {

    },
  },
}
</script>

<style scoped>
@import "../css/monthlist.css";
</style>