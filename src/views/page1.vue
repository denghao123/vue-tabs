<template>
   <div>
     <h1>首页</h1>
     <h4>最近一次刷新：{{update}}</h4> <br>
     <button @click="openTab('page4','关于')">打开"关于"</button>
     <button @click="closeTab('page4')">关闭"关于"</button>
     <button @click="closeTab()">关闭"首页"以外的所有标签</button>
   </div>
</template>
<script>
import lib from "../assets/js/lib.js"
export default {
  props: ['refresh'],
  data() {
    return {
      update: ""
    }
  },
  activated: function() {
    if (this.refresh) {
      this.init();
    }
  },
  methods: {
    init() {
      this.update = lib.dateFormat(new Date(), "hh:mm:ss");
    },
    openTab(key,label){
      this.$emit("childcreatetab",key,label)
    },
    closeTab(type){
      this.$emit("childclosetab",type)
    }
  },
  components: {}
}
</script>