<template>
  <div class="container">
    <Myheader></Myheader>
    <Mybody
      :songList="songList"
      :imageUrl="imageUrl"
      :reviewList="reviewList"
      :isshow="isshow"
      :isplaying="isplaying"
      :mvUrl="mvUrl"
    ></Mybody>
    <Myfooter :url="url" :isplaying="isplaying"></Myfooter>
  </div>
</template>

<script>
import Myheader from "./components/Myheader.vue";
import Mybody from "./components/Mybody.vue";
import Myfooter from "./components/Myfooter.vue";
export default {
  name: "App",
  data() {
    return {
      name: "",
      songList: [],
      url: "",
      imageUrl: "",
      reviewList: [],
      isplaying: "",
      mvUrl: "",
      isshow: false,
    };
  },
  methods: {
    // 获得歌曲的名字
    getSongs() {
      var ply = document.querySelectorAll(".ply");
      for (var i = 0; i < ply.length; i++) {
        ply[i].classList.remove("playMusic");
        ply[i].nextSibling.classList.remove("selectColor");
      }

      let that = this;
      this.$axios
        .get("https://autumnfish.cn/search?keywords=" + this.name)
        .then(
          function (response) {
            that.songList = response.data.result.songs;
          },
          function (error) {
            console.log(error);
          }
        );
    },
    //获得歌曲的在线url地址
    getSing(id, e) {
      var ply = document.querySelectorAll(".ply");
      for (var i = 0; i < ply.length; i++) {
        ply[i].classList.remove("playMusic");
        ply[i].nextSibling.classList.remove("selectColor");
      }
      e.currentTarget.classList.add("playMusic");
      e.currentTarget.nextSibling.classList.add("selectColor");

      this.isshow = false;
      let that = this;
      this.$axios.get("https://autumnfish.cn/song/url?id=" + id).then(
        function (response) {
          that.url = response.data.data[0].url;
        },
        function (error) {
          console.log(error);
        }
      );
      this.getImage(id);
      this.getReviews(id);
    },
    //获取歌曲封面
    getImage(id) {
      let that = this;
      this.$axios.get("https://autumnfish.cn/song/detail?ids=" + id).then(
        function (response) {
          that.imageUrl = response.data.songs[0].al.picUrl;
        },
        function (error) {
          console.log(error);
        }
      );
    },
    //获取歌曲评论
    getReviews(id) {
      let that = this;
      this.$axios.get("https://autumnfish.cn/comment/hot?type=0&id=" + id).then(
        function (response) {
          that.reviewList = response.data.hotComments;
        },
        function (error) {
          console.log(error);
        }
      );
    },
    //控制封面旋转
    play() {
      this.isplaying = true;
    },
    pause() {
      this.isplaying = false;
    },
    //获取MV
    getMv(id) {
      let that = this;
      this.$axios.get("https://autumnfish.cn/mv/url?id=" + id).then(
        function (response) {
          that.isshow = true;
          that.mvUrl = response.data.data.url;
        },
        function (error) {
          console.log(error);
        }
      );
      document.getElementById("audioClass").pause();
    },
  },
  mounted() {
    let that = this;

    // 接受搜索框传输的name并调用getSongs()
    this.$bus.$on("deliveryName", (data) => {
      this.name = data;
      this.getSongs();
    });
    // 当播放键被点击后将接收该id并调用this.getSing()
    this.$bus.$on("songListId", (id, e) => {
      this.getSing(id, e);
    });
    // 当视频按键被点击后接收该id并调用this.getMv()
    this.$bus.$on("mvId", (id) => {
      this.getMv(id);
    });
    // 接收 true 或 false 控制图片旋转
    this.$bus.$on("spin", (data) => {
      this.isplaying = data;
    });

    this.$axios.get("https://autumnfish.cn/search?keywords=beyond").then(
      function (response) {
        that.songList = response.data.result.songs;
      },
      function (error) {
        console.log(error);
      }
    );
  },
  components: {
    Myheader,
    Mybody,
    Myfooter,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

body {
  background-color: #f4f4f4;
}

ul,
li {
  list-style: none;
  text-decoration: none;
}

.container {
  margin-top: 20px;
  padding: 0 !important;
  border: 1px solid #4ca1af;
  background-color: white;
}
</style>