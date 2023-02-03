<template>
  <div class="v-viewer">
    <div class="img-content">
      <template v-for="(item, index) in imgData">
        <img
          :src="item.imgPath"
          @click="previewImg(item)"
          :key="item + index"
          v-viewer="viewerOptions"
        />
      </template>
    </div>
  </div>
</template>

<script>
import img1 from "@/assets/logo.png";
import img2 from "@/assets/aaa.png";
import img3 from "@/assets/more.png";

export default {
  data() {
    return {
      imgData: [
        {
          imgPath: img1,
        },
        {
          imgPath: img2,
        },
        {
          imgPath: img3,
        },
      ],
      viewerOptions: {
        backdrop: true, // 是否启用播放模态背景[布尔值/字符串：默认true](不启用时，点击空白处不会退出播放，仅点击右上角关闭按钮才会退出播放)
        button: true, // 显示右上角关闭按钮[布尔值：默认true]
        navbar: false, // 显示缩略图导航[布尔值/数字：默认true]
        toolbar: true, // 显示工具栏、自定义工具栏[布尔值/数字/对象：默认true]
        tooltip: true, // 显示缩放百分比[布尔值：默认true]
        movable: false, // 图片是否可移动[布尔值：默认true]
        zoomable: true, // 图片是否可缩放[布尔值：默认true]
        rotatable: true, // 图片是否可旋转[布尔值：默认true]
        scalable: false, // 图片是否可翻转[布尔值：默认true]
        transition: true, // 使用CSS3过渡[布尔值：默认true]
        fullscreen: true, // 播放时是否全屏[布尔值/全屏配置：默认true]
        title: false,
        play: [false],
        url: "src", // 播放时图片地址 URL 来源。[字符串/方法：默认src](如果是字符串，则应该是每个图像元素的属性之一。 如果是方法，则应该返回一个有效的图像URL)
      },
    };
  },
  methods: {
    previewImg(val) {
      console.log("val======", val);
    },
  },
};
</script>

<style>
.img-content {
  display: flex;
}
img {
  width: 100px;
}
</style>
