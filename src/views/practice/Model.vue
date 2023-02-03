<template>
  <div class="practice-model">
    <div>Vue2中的model选项</div>
    <Child v-model="num"></Child>
  </div>
</template>
<script>
import Child from "./components/Child.vue";
export default {
  components: {
    Child,
  },
  data() {
    return {
      num: 10,
    };
  },
  methods: {},
};
</script>

<style></style>
