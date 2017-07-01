<template>
   <div>
     <h1>关于</h1>
     <h4>最近一次刷新：{{update}}</h4>
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
    }

  },
  components: {}
}
</script>
