<template>
  <div
    class="musicHeader d-flex justify-content-between align-items-center px-5"
  >
    <h3 class="f-family">网抑云音乐</h3>
    <input
      type="text"
      class="searchIpt form-control mr-5"
      v-model="sname"
      @keyup.enter="search"
      placeholder="music / singer"
    />
  </div>
</template>

<script>
export default {
  sname: "Myheader",
  data() {
    return {
      sname: "",
    };
  },
  methods: {
    search() {
      this.$bus.$emit("deliveryName", this.sname);
    },
  },
};
</script>

<style>
.musicHeader {
  width: 100%;
  height: 70px;
  color: white;
  background: #8e9eab;
  background: linear-gradient(to bottom, #8e9eab, #eef2f3);
}

.musicHeader h3 {
  margin: 0;
}

.f-family {
  font-family: "方正舒体";
}

.searchIpt {
  width: 160px !important;
  height: 32px !important;
  font-size: 12px !important;
  border-radius: 30px !important;
  border: 0;
  background-color: #cedeea !important;
}
</style>