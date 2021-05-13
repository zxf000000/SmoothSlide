<template>
  <div>
    <h1>Audio</h1>
    <audio controls ref="audio">
      <source src="@/assets/hold.mp3" type="audio/mp3">
      Your browser does not support the <code>audio</code> element.
    </audio>
  </div>
</template>

<script>
export default {
  name: "Audio",
  mounted() {
    const AudioContext = window.AudioContext || window.webkitAudioContext;
    const ctx = new AudioContext();
    const analyser = ctx.createAnalyser();
    analyser.fftSize = 512;
    const source = ctx.createMediaElementSource(this.$refs.audio);
    // 将音频源关联到分析器
    source.connect(analyser);
    analyser.connect(ctx.destination);


  }
}
</script>

<style scoped>

</style>
