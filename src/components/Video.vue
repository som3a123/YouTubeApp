<template>
  <div class="video">
    <youtube v-if="isMobile()" :video-id="this.$route.params.videoId" width="100%" height="300px"
    ref="youtube"></youtube>
    <youtube v-else :video-id="this.$route.params.videoId" width="100%" height="500px"
    ref="youtube"></youtube>
    <div class='video_details'>
      <h2 class='video_details_title' v-if="this.$store.state.videoDetails.items">
      {{this.$store.state.videoDetails.items[0].snippet.title}} </h2>
      <div class='video_stats'>
        <label class='views' v-if="this.$store.state.videoDetails.items">
          {{this.$store.state.videoDetails.items[0].statistics.viewCount}} views •
          {{this.$store.state.videoDetails.items[0].snippet.publishedAt}}
        </label>
        <label class='likes' v-if="this.$store.state.videoDetails.items">
          <img class='video_like' src="../assets/like.png"/>
          {{this.$store.state.videoDetails.items[0].statistics.likeCount}}
          <img class='video_like' src="../assets/dislike.png"/>
          {{this.$store.state.videoDetails.items[0].statistics.dislikeCount}}
        </label>
      </div>
    </div>
    <SearchResults/>
  </div>
</template>

<script>
import SearchResults from './SearchResults.vue';

export default {
  name: 'Video',
  components: {
    SearchResults,
  },
  props: {
    desc: String,
    image: String,
    title: String,
    channel: String,
  },
  methods: {
    isMobile() {
      if (/Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)) {
        return true;
      }
      return false;
    },
  },
  mounted() {
    this.$store.dispatch('getRelated', this.$route.params.videoId);
    this.$store.dispatch('getVideoDetails', this.$route.params.videoId);
  },
  updated() {
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.video{
    .iframe{
      margin: 0 auto;
    }
    .video_details{
      flex:2;
      display:flex;
      flex-direction:column;
      align-items: flex-start;
      text-align: left;
      padding: 5px;
      border-bottom: 1px solid lightgrey;
      > *{
        text-decoration: none;
        font-family: verdana;
        color: grey;
      }
      .video_details_title{
        text-align: left;
        font-size: 16px;
        font-weight: bold;
        color: black;
        margin-bottom: 10px;
      }
      .video_stats{
        display:block;
        width:100%;
        padding: 20px 0;
        .views{
          float: left;
          padding: 10px 0;
        }
        .likes{
          padding: 10px 0;
          float: left;
          display:flex;
          align-items:center;
        }
      }
      .result_details_channel{
        font-size: 12px;
      }
      .video_like{
        width:20px;
        margin: 0 10px;
      }
    }
}

$breakpoint-tablet: 768px;
$breakpoint-desktop: 1024px;
@media (min-width: $breakpoint-desktop) {
.video{
    margin: 10px auto;
    padding: 80px 5px 0 5px;
    .video_details{
      padding: 20px 10px;
      .video_details_title{
        font-size: 18px;
      }
      .video_stats{
        display:inline-block;
        width:100%;
        .likes{
          float: right;
        }
      }
    }
}
}

</style>
