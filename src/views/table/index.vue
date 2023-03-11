<template>
  <div class="home">
    <div>
      <h3>遇到问题：</h3>
      打开预览弹出，子组件中video标签中的src已经更新，视频却还是显示原来的视频，并未更新。
    </div>
    <div>
      <h3>原因：</h3>
      参考博客https://blog.csdn.net/Z_0926/article/details/108784419;<br />
      当
      video中存在source标签的时候，浏览器渲染之后会自动去获取地址，即便地址改变，浏览器也不会再去获取地址。
    </div>
    <div>
      <h3>解决方法：</h3>
      动态渲染video标签中的src,实现打开弹出预览视频
    </div>
    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="date" label="日期" width="180"> </el-table-column>
      <el-table-column prop="name" label="姓名" width="180"> </el-table-column>
      <el-table-column prop="address" label="地址"> </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button size="mini" @click="handlePreview(scope.row)"
            >预览</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <preview
      :dialogVisible="visable"
      :url="url"
      @close="close"
      ref="preview"
    ></preview>
  </div>
</template>

<script>
import preview from "./preview.vue";

export default {
  name: "Home",
  components: { preview },
  data() {
    return {
      tableData: [
        {
          date: "2016-05-02",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
          url: "https://vd2.bdstatic.com/mda-mdkr66c7vha52bva/sc/cae_h264/1618998590/mda-mdkr66c7vha52bva.mp4?v_from_s=hkapp-haokan-hna&auth_key=1678464309-0-0-ee5d4add77208629f4172cfdda54b645&bcevod_channel=searchbox_feed&pd=1&cd=0&pt=3&logid=2108895667&vid=3596152173355264163&abtest=107353_1&klogid=2108895667",
        },
        {
          date: "2016-05-04",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1517 弄",
          url: "https://stageandmu.reservehemu.com:8165/oss-file/mclz-test/config-picture/79e0db7c9a4a4f259933acc44ef0bd13?e=1678446439&token=vBeW1pJxKvC4uPjLv5wSbldTP0GTbqmLKINBNRiL:POMb4a2moz0VepYgncsS0rTiUn8=&bucketName=inai-test",
        },
        {
          date: "2016-05-01",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1519 弄",
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
          url: "https://vd2.bdstatic.com/mda-jjviu5ja47d29uki/sc/mda-jjviu5ja47d29uki.mp4?v_from_s=hkapp-haokan-hna&auth_key=1678519535-0-0-255991d2ab060e2ca6bb7b0a4912c59b&bcevod_channel=searchbox_feed&pd=1&cd=0&pt=3&logid=3335506152&vid=9903590133909571389&abtest=107353_1&klogid=3335506152",
        },
      ],
      visable: false,
      url: "",
    };
  },
  methods: {
    handlePreview(val) {
      this.visable = true;
      const aa = val.url.replace("https://", "");
      this.url = aa;
    },
    close() {
      this.visable = false;
    },
  },
};
</script>
