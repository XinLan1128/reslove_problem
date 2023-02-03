<template>
  <div class="drag" ref="dragRef">
    <!-- 仅在编辑状态下才展示四个点 
      1.先暂时默认展示四个点
      2.给这四个点添加拖拽的方法
    -->
    <i
      class="spot spot1"
      v-show="isEdit"
      ref="spotOne"
      :style="{ top: `${spotOneTop}px`, left: `${spotOneLeft}px` }"
    ></i>
    <i
      class="spot spot2"
      v-show="isEdit"
      ref="spotTwo"
      :style="{ top: `${spotTwoTop}px`, left: `${spotTwoLeft}px` }"
    ></i>
    <i
      class="spot spot3"
      v-show="isEdit"
      ref="spotThree"
      :style="{ top: `${spotThreeTop}px`, left: `${spotThreeLeft}px` }"
    ></i>
    <i
      class="spot spot4"
      v-show="isEdit"
      ref="spotFour"
      :style="{ top: `${spotFourTop}px`, left: `${spotFourLeft}px` }"
    ></i>
    <canvas ref="canvas_new"> 您的浏览器不支持，请更新浏览版本 </canvas>
  </div>
</template>
<script>
export default {
  name: "drag",
  props: {
    isEdit: {
      type: Boolean,
      default: false,
    },
    width: {
      type: Number,
      default: 0,
    },
    height: {
      type: Number,
      default: 0,
    },
    x: {
      type: Number,
      default: 100,
    },
    y: {
      type: Number,
      default: 100,
    },
    coordinates: {
      type: Array,
    },
  },
  watch: {
    isEdit() {
      this.draw();
    },
  },
  data() {
    return {
      spotOneTop: 0,
      spotOneLeft: 0,
      spotTwoTop: 0,
      spotTwoLeft: 0,
      spotThreeTop: 0,
      spotThreeLeft: 0,
      spotFourTop: 0,
      spotFourLeft: 0,
      // 判断画的边界是否合理
      allowSave: true,
    };
  },
  methods: {
    // 默认先选择区域全屏
    full() {
      // 把整个区域都覆盖住
      this.spotOneTop = 0;
      this.spotOneLeft = 0;
      this.spotTwoTop = 0;
      this.spotTwoLeft = this.width;
      this.spotThreeTop = this.height;
      this.spotThreeLeft = this.width;
      this.spotFourTop = this.height;
      this.spotFourLeft = 0;
    },
    // 将画布的四个点进行画，定位
    isDeFault() {
      // 1. 按比例1000来换算的,先确定点的位置
      const partX = this.width / this.x;
      const partY = this.height / this.y;
      // 如果当前没有传递对应的coordinates，就默认先选择区域全屏
      if (this.coordinates && this.coordinates.length === 4) {
        this.spotOneTop = Math.floor(partX * this.coordinates[0].point_x);
        this.spotOneLeft = Math.floor(partY * this.coordinates[0].point_y);
        this.spotTwoTop = Math.floor(partX * this.coordinates[1].point_x);
        this.spotTwoLeft = Math.floor(partX * this.coordinates[1].point_y);
        this.spotThreeTop = Math.floor(partX * this.coordinates[2].point_x);
        this.spotThreeLeft = Math.floor(partX * this.coordinates[2].point_y);
        this.spotFourTop = Math.floor(partX * this.coordinates[3].point_x);
        this.spotFourLeft = Math.floor(partX * this.coordinates[3].point_y);
      } else {
        this.full();
      }
      // 2. 画对应的位置
      this.draw();
    },
    // 在canvas进行的画
    draw() {
      // 1.获取节点的，设置对应的画布宽高
      const canvas = this.$refs.canvas_new;
      canvas.width = this.width;
      canvas.height = this.height;
      const ctx = canvas.getContext("2d");
      // 没有上下文，就不执行
      if (!ctx) return;
      // 绘制路径(闭合)----https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes
      // 每次重新绘制都要清空当前画布内容
      ctx.clearRect(0, 0, this.width, this.height);
      ctx.beginPath();
      // 2. 将画笔移到第一点上面 ,x和y顺序不要弄反
      ctx.moveTo(this.spotOneLeft, this.spotOneTop);
      ctx.lineTo(this.spotTwoLeft, this.spotTwoTop);
      ctx.lineTo(this.spotThreeLeft, this.spotThreeTop);
      ctx.lineTo(this.spotFourLeft, this.spotFourTop);
      ctx.closePath();
      // 3. 通过编不编辑来显示不同的颜色
      if (this.isEdit) {
        if (this.allowSave) {
          ctx.fillStyle = "rgba(64, 158, 255, 0.6)";
        } else {
          ctx.fillStyle = "rgba(255, 0, 0, 0.3)";
        }
      } else {
        ctx.fillStyle = "rgba(255, 0, 0, 0.4)";
      }
      // 4. 填充成实心图形
      ctx.fill();
    },
    xx(p1, p2) {
      return p1.x * p2.y - p2.x * p1.y;
    },
    // 判断两条线段是否相交，<0 p2在p1的顺时针方向，>0 p2在p1的逆时针
    iv(p1, p2) {
      return { x: p2.x - p1.x, y: p2.y - p1.y };
    },
    // 边界判断---此区域是否能绘制
    isConcave() {
      // 拿到对应的点坐标
      const A = { x: this.spotOneLeft, y: this.spotOneTop };
      const B = { x: this.spotTwoLeft, y: this.spotTwoTop };
      const C = { x: this.spotThreeLeft, y: this.spotThreeTop };
      const D = { x: this.spotFourLeft, y: this.spotFourTop };
      // 将点连接成线
      const AB = this.iv(A, B);
      const AC = this.iv(A, C);
      const AD = this.iv(A, D);
      const BC = this.iv(B, C);
      const BD = this.iv(B, D);
      // 判断对应线的位置是否边界合理
      const w = this.xx(AB, AC);
      const x = this.xx(AB, AD);
      const y = this.xx(AC, AD);
      const z = this.xx(BC, BD);
      // 只要有一个相交都是不能保存成功
      return (
        (x < 0 && y < 0 && z < 0 && w < 0) || (x > 0 && y > 0 && z > 0 && w > 0)
      );
    },
  },
  mounted() {
    this.isDeFault();
    const { dragRef } = this.$refs;
    // 在mounted状态下，获取对应的spot节点添加对应的方法
    const spotOne = this.$refs.spotOne;
    const spotTwo = this.$refs.spotTwo;
    const spotThree = this.$refs.spotThree;
    const spotFour = this.$refs.spotFour;
    // 给当前节点添加鼠标的按下mousedown和抬起mouseup
    [spotOne, spotTwo, spotThree, spotFour].forEach((item, index) => {
      // 某个节点的鼠标按下事件
      item.onmousedown = () => {
        if (!this.isEdit) return;
        // 当前画布的宽高，位置定位值
        const clientRect = dragRef.getBoundingClientRect();
        // 鼠标移动
        document.onmousemove = (e) => {
          const endX = Math.floor(e.clientX - clientRect.left);
          const endY = Math.floor(e.clientY - clientRect.top);
          // 判断对应的临界值后，鼠标移动后坐标点的再重新设置 x 和 y值
          if (endX >= 0 && endX <= this.width) {
            // 判断当前是哪个点
            switch (index) {
              case 0:
                this.spotOneLeft = endX;
                break;
              case 1:
                this.spotTwoLeft = endX;
                break;
              case 2:
                this.spotThreeLeft = endX;
                break;
              case 3:
                this.spotFourLeft = endX;
                break;
              default:
                break;
            }
          }
          if (endY >= 0 && endY <= this.height) {
            switch (index) {
              case 0:
                this.spotOneTop = endY;
                break;
              case 1:
                this.spotTwoTop = endY;
                break;
              case 2:
                this.spotThreeTop = endY;
                break;
              case 3:
                this.spotFourTop = endY;
                break;
              default:
                break;
            }
          }
          this.allowSave = this.isConcave();
          this.draw();
        };
        // 鼠标抬起--将移动事件置空
        document.onmouseup = () => {
          document.onmousemove = null;
          console.log("dddd");
        };
      };
    });
    // 给对应画布的编辑判断
  },
};
</script>

<style>
.drag {
  height: inherit;
  width: inherit;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
}
/* 
  spot:公共样式
  spot1,spot2,spot3,spot4:当前未传值的默认位置
  通过权重值的不同，内联样式>外联样式
*/
.spot {
  display: inline-block;
  width: 10px;
  height: 10px;
  border: 2px solid white;
  background: rgb(64, 158, 255);
  border-radius: 50%;
  cursor: pointer;
  position: absolute;
  /* 给元素添加移动效果 */
  transform: translate(-50%, -50%);
}
.spot2 {
  top: 0;
  left: 100%;
}
.spot3 {
  top: 100%;
  left: 100%;
}
.spot4 {
  top: 100%;
  left: 0;
}
</style>
