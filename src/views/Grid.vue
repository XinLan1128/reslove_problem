<template>
  <div class="container">
    <div class="practice">
      <div class="img">
        <div
          v-for="item in rows * coloums"
          @click="hanldeItem(item)"
          :key="item"
          :class="{ active: actives.includes(item.toString()) }"
          :disabled="!isEdit"
        >
          {{ item }}
        </div>
      </div>
    </div>
    <div class="btns" v-if="!isEdit">
      <el-button type="primary" @click="handleEdit">编辑</el-button>
    </div>
    <div class="btns" v-else>
      <el-button type="primary" @click="handleFinsh">完成</el-button>
      <el-button type="primary" @click="handleCancel">取消</el-button>
    </div>
  </div>
</template>

<script>
export default {
  name: "grid",
  props: {
    rows: {
      type: Number,
      default: 5,
    },
    coloums: {
      type: Number,
      default: 5,
    },
    // isEdit: {
    //   type: Boolean,
    //   default: false,
    // },
    defaultActions: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      actives: [],
      isEdit: false,
    };
  },
  methods: {
    hanldeItem(val) {
      // 当前的actives的数组值为['1','2','3','4','5','6','7',...,'24','25'],对应值的下标均要比值-1
      // 点击时得到的是比actives下标多1的下标值
      if (this.isEdit) {
        if (this.actives[val - 1]) {
          this.actives[val - 1] = undefined;
        } else {
          // 当前没有这个值就用的
          this.actives[val - 1] = val.toString();
        }
        this.actives = [...this.actives];
      } else {
        this.$message({
          showClose: true,
          message: "请先点击“编辑”按钮，再进入编辑区域",
          type: "warning",
        });
      }
    },
    handleDefault() {
      this.actives = new Array(this.rows * this.coloums);
      for (let i = 0; i < this.actives.length; i += 1) {
        this.actives[i] = `${i + 1}`;
      }
    },
    handleEdit() {
      this.isEdit = true;
      this.actives = this.defaultActions;
    },
    handleFinsh() {
      this.isEdit = false;
      console.log("当前选择的区域====", this.actives);
    },
    handleCancel() {
      this.isEdit = false;
      this.handleDefault();
    },
  },
  created() {
    this.handleDefault();
  },
};
</script>

<style>
.container {
  display: flex;
  align-items: flex-end;
}
.practice {
  width: 400px;
  border: 1px solid red;
  background: url(../assets/more.png) repeat;
}
.img {
  height: 100%;
  display: grid;
  grid-template-columns: auto auto auto auto auto;
  grid-template-rows: auto auto auto auto auto;
  /* gap: 20px 50px; */
  /* 只指定了第一行高度,第一列的宽度 */
  /* grid-template-rows: 20px; */
  /* grid-template-columns: 20px; */

  /* 设置网格中列的默认值 每个元素都会有效果 */
  /* grid-auto-columns: auto; */
  /* grid-auto-rows: auto; */

  /* 
     grid-area属性，在其后的元素会自动填满跨过的元素；grid-column属性是第几列开始，第几列结束，会空出对应跨越的位置
     grid-area: grid-row-start(哪一行开始显示) / grid-column-start(哪一列开始显示) / grid-row-end (跨越多少行) / grid-column-end (跨越多少列)
     grid-template-areas: 'myArea myArea myArea myArea myArea';
     
  */
  /* grid-template-areas: "myArea myArea . . ."; */

  /* 栅格中的自动布局，按行或者按列等，在给某个特定的元素位置后，其他元素会自动填充 */
  /* grid-auto-flow: row dense; */
  grid-template-areas: "a1 a2 a3 a4 a5", "a6 a7 a8 a9 a10",
    "a11 a12 a13 a14 a15", "a16 a17 a18 a19 a20", "a21 a22 a23 a24 a25";
}
.item3 {
  /* grid-area: 2 / 1 / span 2 / span 3; */
  /* grid-column: auto / span 4; */
}
.btns {
  margin-left: 20px;
}
.img > div {
  text-align: center;
  border: 1px solid red;
  line-height: 80px;
}
div > .active {
  background-color: rgba(255, 0, 0, 0.2);
}
</style>
