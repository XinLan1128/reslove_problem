<template>
  <div class="video-player" id="playerId"></div>
</template>

<script>
/* eslint-disable */ //
export default {
  name: "VideoPlayer",
  data() {
    return {
      player: null
    };
  },
  // mounted() {
  //   this.hls_load();
  // },
  methods: {
    hls_load(playAddress) {
      const element = document.getElementById('playerId');
      if (this.player) {
        this.player.destroy();
        this.player = null;
      }
      if (Hls.isSupported()) {
        const conf = Hls.DefaultConfig;
        conf.debug = true;// 设置log
        Hls.DefaultConfig = conf;
        // const start = parseInt(document.getElementsByName('startPosition')[0].value);
        this.player = new Hls({
          startPosition: -1,
          resolutionAdaption: true,
          suppressNoise: 0
        });
        // player = new Hls();
        this.player.attachMedia(element);
        this.player.addEventListener('pause', () => {
          console.log("listen paused");
        });
        this.player.addEventListener('waiting', () => {
          console.warn(`listen waiting at ${this.player.currentTime}`);
        });
        this.player.addEventListener('stalled', () => {
          console.warn("listen stalled");
        });
        this.player.addEventListener('canplay', () => {
          console.warn('listen canplay');
        }, { once: true });
        this.player.addEventListener('ended', () => {
          console.warn("listen ended");
        });
        this.player.on(Hls.Events.STATISTICS_INFO, (event, info) => {
          if (!info.realtimeFps) { return; }
          console.log('实时帧率');
        });
        this.player.on(Hls.Events.FRAG_PARSING_INIT_SEGMENT, () => {
          console.log('mediaInfo');
          console.log(this.player.mediaInfo);
        });
        this.player.on(Hls.Events.MEDIA_ATTACHING, () => {
          this.player.getInnerElement().element.controls = false;
        });
        this.player.on(Hls.Events.ERROR, (type, e) => {
          console.error(type, e);
        });
        this.player.loadSource(playAddress);
      }
    },
    flv_load(playAddress) {
      console.log("当前的播放连接", playAddress);
      var element = document.getElementById("playerId");
      var mediaDataSource = {
        type: "flv",
        cors: true
      };
      mediaDataSource["url"] = playAddress;

      if (this.player) {
        this.player.destroy();
        this.player = null;
      }
      this.player = flvjs.createPlayer(mediaDataSource, {
        separateAV: true,
        fixAudioTimestampGap: false,
        enablePlaybackRateAdaption: true,
        resolutionAdaption: false
      });

      this.player.attachMediaElement(element, "clear:both;display:block;margin:auto");

      this.player.on(flvjs.Events.MEDIA_ATTACHING, () => {
        this.player.getInnerElement().element.controls = true;
        this.player.addEventListener("waiting", () => {
          console.warn("listen waiting");
        });
        this.player.addEventListener("playing", () => {
          console.warn("listen playing");
        });
        this.player.addEventListener("ended", () => {
          console.warn("listen ended");
        });
        this.player.addEventListener("pause", () => {
          console.warn("listen pause");
        });
        this.player.addEventListener("error", e => {
          console.log(e);
        });
        this.player.addEventListener("ratechange", () => {
          if (this.player) console.log("speed:" + this.player.playbackRate);
        });
      });
      this.player.on(flvjs.Events.CODEC_SWITCHING, data => {
        console.warn(`listen codec switching from ${data.from} to ${data.to}`);
      });
      this.player.load(playAddress);
    },
    mp4_load(playAddress){
      const element = document.getElementById('playerId');
      if (this.player) {
        this.player.destroy();
        this.player = null;
      }
        this.player = mp4js.createPlayer({lazyLoadMaxDuration:120});//详细config参见word文档
        this.player.attachElement(element);
        this.player.on('media_attaching',()=>{this.player.getInnerElement().element.controls=true});
        this.player.on('error',(type, info)=>{
            console.log(type, info);
        });
        this.player.addEventListener('pause',()=>{console.warn('listen pause')});
        this.player.addEventListener('ended',()=>{console.warn('listen ended')});
        this.player.addEventListener('waiting',()=>{console.warn('listen waiting')});
        this.player.load(playAddress);
    },
    loadSource(playAddress) {
      if (this.player) {
        this.player.loadSource(playAddress);
      }
    }
  }
};
</script>

<style scoped>
.video-player {
  width: 100px;
  height: 100px;
}
</style>
