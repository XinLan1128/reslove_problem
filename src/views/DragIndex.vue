<template>
  <div class="drag_index">
    <div class="drag_index_mask">
      <div class="drag_index_img"></div>
      <Grag
        :isEdit="isEdit"
        :width="400"
        :height="225"
        :x="1000"
        :y="1000"
      ></Grag>
    </div>
    <div class="drag_index_btns" v-if="isEdit">
      <el-button type="primary" @click="handleFinish">完成</el-button>
      <el-button type="primary" @click="handleCancel">取消</el-button>
    </div>
    <div class="drag_index_btns" v-else>
      <el-button type="primary" @click="handleEdit">编辑</el-button>
    </div>
  </div>
</template>

<script lang="js">
import Grag from "./Drag.vue";
export default {
  name: "dragIndex",
  components: {
    Grag,
  },
  data() {
    return {
      isEdit: false,
    };
  },
  methods: {
    handleEdit() {
      this.isEdit = true;
    },
    handleCancel() {
      this.isEdit = false;
    },
    handleFinish() {
      this.handleCancel();
    },
  },
};
</script>

<style>
.drag_index {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: flex-end;
  /* background: saddlebrown; */
}
.drag_index_mask {
  width: 400px;
  height: 225px;
  position: relative;
  border: 1px solid red;
}
.drag_index_img {
  height: inherit;
  width: inherit;
  background: url(../assets/ddd.webp);
}
.drag_index_btns {
  margin-left: 20px;
}
</style>
