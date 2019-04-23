<template>
 <video id="music" ref="player" controls uk-cover></video>
</template>

<script>
export default {
  name: 'player',

  props: {
    track: {
      type: String,
      default: "",
      required: true
    }
  },

  watch: {
    track(val, oldVal) {
      this.$refs.player.src = val
      this.$refs.player.play()
    }
  }
}
</script>
<style scope>

video {
    width: 100%;
    height: 50px;
}
</style>