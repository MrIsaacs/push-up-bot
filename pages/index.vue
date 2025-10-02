<template>
  <div class="container">
    <div>Teachable Machine Pose Model</div>
    <button type="button" onclick="init()">Start</button>
    <div>
      <canvas ref="canvas" id="canvas" />
    </div>
    <div ref="labelContainer" id="label-container" />
  </div>
</template>

<script>
import * as tf from '@tensorflow/tfjs'
import * as tmPose from '@teachablemachine/pose'
// import checkpointURL from '~/assets/push-up-model/model.json'
// import metadataURL from '~/assets/push-up-model/metadata.json'

export default {
  data: () => ({
    URL: '/',
    checkpointURL: 'model.json',
    metadataURL: 'metadata.json',
    model: null,
    webcam: null,
    ctx: null,
    labelContainer: null,
    maxPredictions: null,
  }),
  async mounted() {
    // load the model and metadata
    // Refer to tmImage.loadFromFiles() in the API to support files from a file picker
    // Note: the pose library adds a tmPose object to your window (window.tmPose)
    this.model = await tmPose.load(this.checkpointURL, this.metadataURL);
    console.log(this.checkpointURL, this.metadataURL);
    this.maxPredictions = this.model.getTotalClasses();

    // Convenience function to setup a webcam
    const size = 200;
    const flip = true; // whether to flip the webcam
    this.webcam = new tmPose.Webcam(size, size, flip); // width, height, flip
    await this.webcam.setup(); // request access to the webcam
    await this.webcam.play();
    window.requestAnimationFrame(loop);

    // append/get elements to the DOM
    const canvas = this.$refs.canvas;
    canvas.width = size;
    canvas.height = size;
    this.ctx = canvas.getContext('2d');
    labelContainer = this.$refs.labelContainer;
    for (let i = 0; i < maxPredictions; i++) {
      // and class labels
      labelContainer.appendChild(document.createElement('div'));
    }
  },
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
