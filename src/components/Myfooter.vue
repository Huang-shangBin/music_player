<template>
  <div class="audioFooter">
    <audio
      :src="url"
      controls
      autoplay
      @play="play"
      @pause="pause"
      id="audioClass"
    ></audio>
  </div>
</template>

<script>
export default {
  props: ["url", "isplaying"],
  methods: {
    play() {
      this.$bus.$emit("spin", true);
    },
    pause() {
      this.$bus.$emit("spin", false);
    },
  },
};
</script>

<style>
.audioFooter {
  border-top: 1px solid #ccc;
  background-color: #f1f3f4;
}

#audioClass {
  display: block;
  width: 100%;
  border-radius: 0 !important;
}
</style>