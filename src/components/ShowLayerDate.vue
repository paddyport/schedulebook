<template>
  <div id="ShowDate" class="show">
    <div class="header">
      <GenerHead
        :head-str="markYear+'年'+(markMonth+1)+'月'+markDate+'日'">
      </GenerHead>
      <GenerCclbtn
        :btn-flg="true"
        :btn-cls="'icn nml cls'"
        @click="closeAnew">
      </GenerCclbtn>
    </div>
    <div class="body">
      <div class="switch">
        <GenerTabbtn
          :current-flg="tabCurrent=='prj'"
          :btn-flg="showPrjArr.length ? true : false"
          :btn-str="'スケジュール'"
          :btn-cls="'def nml prj'"
          :btn-submit="'prj'"
          @tabbtn-click="tabSwitch">
        </GenerTabbtn>
        <GenerTabbtn
          :current-flg="tabCurrent=='tsk'"
          :btn-flg="showPrjArr.length ? true : false"
          :btn-str="'タスク'"
          :btn-cls="'def nml tsk'"
          :btn-submit="'tsk'"
          @tabbtn-click="tabSwitch">
        </GenerTabbtn>
      </div>
      <div v-if="tabCurrent=='prj'" class="tab prj">
        <ul v-if="showPrjArr.length">
          <li 
            v-for="(sp, spidx) in showPrjArr"
            :data-pid="sp.pid"
            :key="spidx">
            <h2 class="title">{{ sp.title }}</h2>
            <ul class="member">
              <li v-for="(pm, pmidx) in sp.member" :key="pmidx">
                <figure>
                  <img :src="mmbArr[mmbArr.findIndex((li)=>li.mid==pm)].icon" :alt="mmbArr[mmbArr.findIndex((li)=>li.mid==pm)].name">
                </figure>
              </li>
            </ul>
            <div class="date">
              <p>
                {{ new Date(sp.start).getFullYear() }}年{{ new Date(sp.start).getMonth()+1 }}月{{ new Date(sp.start).getDate() }}日
              </p>
              <p v-if="sp.start!=sp.end">
                {{ new Date(sp.end).getFullYear() }}年{{ new Date(sp.end).getMonth()+1 }}月{{ new Date(sp.end).getDate() }}日
              </p>
            </div>
            <div class="btns">
              <GenerIcnbtn
                :btn-flg="true"
                :btn-cls="'def prj edt'"
                @icnbtn-click="openEditPrj">
              </GenerIcnbtn>
              <GenerIcnbtn
                :btn-flg="true"
                :btn-cls="'def nml cht'"
                @icnbtn-click="openEditPrj">
              </GenerIcnbtn>
              <GenerIcnbtn
                :btn-flg="true"
                :btn-cls="'def alt rem'"
                @icnbtn-click="remove">
              </GenerIcnbtn>
            </div>
          </li>
        </ul>
        <p v-else>何も登録されていません。</p>
      </div>
      <div v-if="tabCurrent=='tsk'" class="tab tsk">
        <ul v-if="showTskArr.length">
          <li 
            v-for="(st, stidx) in showTskArr"
            :data-tid="st.tid"
            :key="stidx">
            <i
              v-if="st.lid"
              class="label"
              :style="{color: lblArr[lblArr.findIndex((li)=>li.lid==st.lid)].color}">
              {{ lblArr[lblArr.findIndex((li)=>li.lid==st.lid)].title }}
            </i>
            <h2 class="title">{{ st.title }}</h2>
            <ul class="member">
              <li v-for="(tm, tmidx) in st.member" :key="tmidx">
                <figure>
                  <img :src="mmbArr[mmbArr.findIndex((li)=>li.mid==tm)].icon" :alt="mmbArr[mmbArr.findIndex((li)=>li.mid==tm)].name">
                </figure>
              </li>
            </ul>
            <div class="date">
              <p :class="new Date(nowYear, nowMonth, nowDate).getTime()>st.end ? 'alt' : ''">
                {{ new Date(st.start).getFullYear() }}年{{ new Date(st.start).getMonth()+1 }}月{{ new Date(st.start).getDate() }}日
              </p>
              <p
                v-if="st.start!=st.end"
                :class="new Date(nowYear, nowMonth, nowDate).getTime()>st.end ? 'alt' : ''">
                {{ new Date(st.end).getFullYear() }}年{{ new Date(st.end).getMonth()+1 }}月{{ new Date(st.end).getDate() }}日
              </p>
            </div>
            <div class="status">
              <p
                v-if="st.status==0"
                :class="['stt0', new Date(nowYear, nowMonth, nowDate).getTime()>st.end ? 'aaltlr' : '']">
                未処理
              </p>
              <p
                v-if="st.status==1"
                :class="['stt1', new Date(nowYear, nowMonth, nowDate).getTime()>st.end ? 'alt' : '']">
                処理中
              </p>
              <p v-if="st.status==2" class="stt2">
                処理済み
              </p>
            </div>
            <div class="btns">
              <GenerIcnbtn
                :btn-flg="true"
                :btn-cls="'def tsk edt'"
                @icnbtn-click="openEditPrj">
              </GenerIcnbtn>
              <GenerIcnbtn
                :btn-flg="true"
                :btn-cls="'def alt rem'"
                @icnbtn-click="remove">
              </GenerIcnbtn>
            </div>
          </li>
        </ul>
        <p v-else>何も登録されていません。</p>
      </div>
    </div>
    <div class="footer">
      <GenerTxtbtn
        :btn-flg="true"
        :btn-str="'新規作成'"
        :btn-cls="'def nml prj'"
        @txtbtn-click="openAnewPrj">
      </GenerTxtbtn>
      <GenerTxtbtn
        :btn-flg="true"
        :btn-str="'新規作成'"
        :btn-cls="'def nml tsk'"
        @txtbtn-click="openAnewTsk">
      </GenerTxtbtn>
    </div>
  </div>
</template>

<script>
import GenerHead from './GenerHead'
import GenerIcnbtn from './GenerIcnbtn'
import GenerCclbtn from './GenerCclbtn'
import GenerTxtbtn from './GenerTxtbtn'
import GenerTabbtn from './GenerTabbtn'

export default {
  name: 'ShowLayerDate',
  props: {
    nowYear: Number,
    nowMonth: Number,
    nowDate: Number,
    markYear: Number,
    markMonth: Number,
    markDate: Number,
    showPrjArr: Array,
    showTskArr: Array,
    mmbArr: Array,
    lblArr: Array,
  },
	data() {
		return {
      tabCurrent: "prj",
      markTime: 0,
		}
  },
  components: {
    GenerHead,
    GenerIcnbtn,
    GenerCclbtn,
    GenerTxtbtn,
    GenerTabbtn,
  },
  created: function(){
    this.nowTime = new Date(this.nowYear, this.nowMonth, this.nowDate).getTime();
    this.tabCurrent = this.showPrjArr.length ? "prj" : "tsk";
  },
  methods: {
    tabSwitch(str) {
      this.tabCurrent = str;
    },
    openMember() {

    },
    openEditPrj(e) {
      this.$emit("ap-shown-loader");
      const pid = e.target.parentNode.parentNode.dataset.pid;
      this.$emit("ap-open-edit-prj", pid);
    },
    remove() {

    },
    openAnewPrj() {
      this.$emit("ap-shown-loader");
      this.$emit("ap-open-mark-anew-prj");
    },
    openAnewTsk() {
      this.$emit("ap-shown-loader");
      this.$emit("ap-open-mark-anew-tsk");
    },
    closeAnew() {
      this.$emit("ap-close-show");
    },
  },
}
</script>

<style scoped>
@import "../css/show.css";
</style>