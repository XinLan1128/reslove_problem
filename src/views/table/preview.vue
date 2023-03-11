<template>
  <div class="home">
    预览
    <el-dialog
      title="提示"
      :visible.sync="dialogVisible"
      width="800px"
      height="400px"
      :before-close="handleClose"
    >
      {{ url }}
      <!-- <player ref="video" class="aaa"></player> -->
      <video
        width="320"
        height="240"
        controls
        ref="video"
        style="display: none; margin: auto"
      >
        <source src="" />
      </video>
    </el-dialog>
  </div>
</template>

<script>
// import player from "./play.vue";

export default {
  name: "Home",
  // components: { player },
  props: {
    dialogVisible: {
      type: Boolean,
      default: false,
    },
    url: {
      type: String,
      default: "",
    },
  },
  watch: {
    url: {
      handler(val) {
        this.$nextTick(() => {
          const dom = this.$refs.video;
          if (val) {
            dom.style.display = "block";
            dom.src = `https://${this.url}`;
          }
        });
      },
      deep: true,
      immediate: true,
    },
  },
  data() {
    return {
      dom: "",
    };
  },
  methods: {
    handleClose() {
      this.$emit("close");
    },
    // play() {
    //   const dom = document.getElementById("video");
    //   console.log("dom", dom);
    //   dom.style.display = "block";
    //   dom.src = `https://${this.url}`;
    // },
  },
};
</script>
<style>
.show {
  display: block;
}
.ishow {
  display: none;
}
</style>
