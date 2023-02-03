<template>
  <div class="grid-index">
    <div class="father-content">父组件</div>
    <div class="son-content">
      子组件
      <Son :rows="5" :coloums="5" :defaultActions="defaultActions"></Son>
    </div>
  </div>
</template>

<script>
import Son from "./Grid.vue";
export default {
  name: "gridIndex",
  components: {
    Son,
  },
  data() {
    return {
      defaultActions: ["1", "3", "6", "7", "8", "9", "10"],
    };
  },
  methods: {},
};
</script>

<style>
.grid-index {
  height: 100%;
  width: 100%;
}
.son-content {
  height: inherit;
  width: inherit;
}
</style>
