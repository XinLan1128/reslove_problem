<template>
  <div class="practice-model-child">
    对应变化的值{{ num }}
    <el-button type="primary" @click="add(5)">+</el-button>
    <el-button type="primary" @click="dec(1)">-</el-button>
  </div>
</template>
<script>
export default {
  model: {
    prop: "num",
    event: "click",
  },
  props: {
    num: {
      type: Number,
      default: 0,
    },
  },
  methods: {
    add(val) {
      const newNum = this.num + val;
      this.$emit("click", newNum);
    },
    dec(val) {
      const newNum = this.num - val;
      this.$emit("click", newNum);
    },
  },
};
</script>

<style></style>
