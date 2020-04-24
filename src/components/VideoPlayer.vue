<template>
<div>
  <video ref="videoPlayer" class="video-js" controls autoplay width="854" height="480"
    data-setup='{ "techOrder": ["youtube"], "sources": [{ "type": "video/youtube", "src": "https://www.youtube.com/embed/b0CX4qBK_fo"}], "youtube": { "customVars" : { } } }'>
  </video>
  <div id="add-marker">
    <button v-on:click="logWhereYouAt">Add BookMark</button>
  </div>
  <div ref="container"></div>
</div>
</template>

<style>
</style>

<script>
import videojs from 'video.js';
import AnnotationComments from '@contently/videojs-annotation-comments';
import ChildVideo from './ChildVideo'
import Vue from 'vue';


export default {
  name: "VideoPlayer",
  components: {
    // ChildVideo
  },
  props: {
    options: {
      type: Object,
      default () {
        return {};
      }
    },
  },
  data() {
    return {
      player: null,
      children: []
    }
  },
  methods: {
    logWhereYouAt: function() {
      var whereYouAt = Math.floor(this.player.currentTime());
      console.log(whereYouAt);
      this.player.markers.add([{
        time: whereYouAt,
        text: 'dummy',
        overlayText: "Marker added at " + whereYouAt
      }])
      this.addChildVideo(whereYouAt);
    },
    addChildVideo: function(whereYouAt) {
      console.log("add child video")
      var l = this.children.length
      var child = {};
      if (l == 0) {
        child = {
          'start': 0,
          'end': whereYouAt
        }

      } else {
        var start = this.children[l - 1]['end']
        child = {
          'start': start,
          'end': whereYouAt
        }
      }
      console.log(child);
      this.children.push(child);
      var ComponentClass = Vue.extend(ChildVideo);
      var instance = new ComponentClass({
        propsData: { start: child.start, end: child.end}
      });
      instance.$mount();
      this.$refs.container.appendChild(instance.$el);
    },
  },
  mounted() {
    this.player = videojs(this.$refs.videoPlayer, this.options, function onPlayerReady() {
      console.log('onPlayerReady', this);
      this.markers({
        breakOverlay: {
          display: true
        },
        markers: []
      });
    })
  },
  beforeDestroy() {
    if (this.player) {
      this.player.dispose()
    }
  }
}
</script>
