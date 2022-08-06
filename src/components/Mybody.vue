<template>
  <div class="main d-flex justify-content-around">
    <div class="leftBox col-3">
      <div class="singListTitle">
        <p>歌曲列表</p>
      </div>
      <div
        v-for="(item, index) in songList"
        :key="index"
        class="songList d-flex align-items-center justify-content-between"
      >
        <div class="d-flex align-items-center justify-content-between">
          <span class="ply" @click="getSing(item.id, $event)"></span>
          <span>{{ item.name }}</span>
          <span
            class="mv"
            @click="getMv(item.mvid)"
            v-if="item.mvid !== 0"
          ></span>
        </div>
      </div>
    </div>
    <div v-if="!imageUrl" class="col-9">
      <img src="../assets/shouye.png" width="100%" height="100%" alt="" />
    </div>
    <template v-if="imageUrl">
      <div
        class="centerBox col-6 d-flex align-items-center justify-content-center"
        v-if="!isshow"
      >
        <img
          :src="imageUrl"
          width="300px"
          :class="{ playing: isplaying }"
          class="cover"
        />
      </div>
      <div v-if="isshow" class="col-9 mt-5">
        <video :src="mvUrl" class="videoBox" autoplay controls></video>
      </div>
      <div class="rightBox col-3" v-if="!isshow">
        <ul class="reviews">
          <h5>热门评论：</h5>
          <div v-for="(review, index) in reviewList" :key="index">
            <div>
              <img :src="review.user.avatarUrl" class="commetImg" />
              {{ review.user.nickname }}:
            </div>
            <p>{{ review.content }}</p>
          </div>
        </ul>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  name: "Mybody",
  props: ["songList", "imageUrl", "reviewList", "isshow", "isplaying", "mvUrl"],
  methods: {
    getSing(id, e) {
      this.$bus.$emit("songListId", id, e);
    },
    getMv(id) {
      this.$bus.$emit("mvId", id);
    },
  },
};
</script>

<style>
.main {
  height: 530px;
  overflow: hidden;
  padding: 10px 0;
}

.leftBox {
  overflow: hidden;
  border-right: 1px solid #ccc;
  /* background: #8e9eab;
  background: linear-gradient(to bottom, #eef2f3, #8e9eab); */
}

.singListTitle {
  padding-bottom: 2px;
  border-bottom: 3px solid #8e9eab;
}

.singListTitle p {
  padding: 0 !important;
  margin: 0 !important;
}

.ply,
.mv {
  cursor: pointer;
  background: url(../assets/table.png) no-repeat 0 9999px;
}

.mv {
  width: 23px;
  height: 17px;
  margin: 2px 0 0 5px;
  background-position: 0px -151px;
}

.ply {
  float: left;
  width: 17px;
  height: 17px;
  background-position: 0 -103px;
  margin-right: 5px;
}

.ply:hover {
  background-position: -0px -128px;
}

.playMusic {
  background-position: -20px -128px !important;
}

.selectColor {
  color: #cc1010;
}

.songList {
  height: 40px;
}

.centerBox {
  border-right: 1px solid #ccc;
}

.videoBox {
  width: 800px;
  height: 450px;
}

.playing {
  transform: rotate(360deg);
  animation: rotation 20s linear infinite;
}

@keyframes rotation {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
}

.rightBox {
  height: 100%;
  overflow: auto;
}

.reviews p {
  font-size: 12px;
  text-indent: 3em;
}

.cover {
  width: 300px;
  height: 300px;
  border-radius: 50%;
  border: 1px solid #ccc;
}

.commetImg {
  width: 30px;
  height: 30px;
  border-radius: 50%;
}
</style>