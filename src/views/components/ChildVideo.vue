<template>
<div>
  start: {{ start }} end: {{ end}}
  <video ref="childVideo" class="video-js" controls autoplay width="854" height="480"
    data-setup='{ "techOrder": ["html", "youtube"],"sources": [{ "type": "video/youtube", "src": "https://www.youtube.com/embed/b0CX4qBK_fo"}],"youtube": { "customVars" : { } } }'>
  </video>
</div>
</template>

<style>
</style>

<script>
import videojs from 'video.js';

export default {
  name: "ChildVideo",
  props: ['start', 'end', 'options'],
  data() {
    return {
      player: null,
    }
  },
  methods: {},
  mounted() {
    this.player = videojs(this.$refs.childVideo, {
      youtube: {
        start: this.start,
        end: this.end
      }
    }, this.options, function onPlayerReady() {
      console.log('onPlayerReady', this);
    })

  },
  beforeDestroy() {
    if (this.player) {
      this.player.dispose()
    }
  }
}
</script>
