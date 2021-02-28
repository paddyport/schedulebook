<template>
  <div 
    id="MonthList"
    :class="['monthlist', monthListFlg ? 'isActive' : '']">
    <ul>
      <li 
        v-for="(ml, mlidx) in monthListArr"
        :class="['day', currentYyMm.yy==ml.yy&&currentYyMm.mm==ml.mm ? 'isCurrent' : '']"
        :key="mlidx">
        <a
          :data-yy="ml.yy"
          :data-mm="ml.mm"
          @click="onClick">
          <span>{{ ml.yy }}年{{ ml.mm+1 }}月</span>
          <small v-if="ml.prj.length">{{ ml.prj.length }}</small>
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'MonthList',
  props: {
    monthListFlg: Boolean,
    currentYyMm: Object,
    monthListArr: Array,
  },
  methods: {
    onClick(e) {
      const yy = Number(e.target.dataset.yy),
        mm = Number(e.target.dataset.mm);
      this.$emit("click-month", yy, mm);
    },
  },
}
</script>

<style scoped>
@import "../css/monthlist.css";
</style>