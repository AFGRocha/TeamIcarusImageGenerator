<template>
  <div class="canvas-container">
    <el-button v-if="showSave" type="primary" @click="save">Save</el-button>
    <canvas ref="myCanvas" width="1674" height="739"></canvas>
  </div>
</template>

<script>
export default {
  name: "Canvas",
  data() {
    return {
      imgBackground: new Image(),
      imgForeground: new Image(),
      imgMask: new Image(),
      showSave: false,
    };
  },
  mounted() {
    this.imgBackground.src = require("../assets/backgrounds/smashnainvicta_background.png");
    this.imgForeground.src = require("../assets/foregrounds/smashnainvicta_foreground.png");
    this.imgMask.src = require("../assets/mask/mask.png");
  },
  methods: {
    generate() {
      // Get the canvas element and its context using $refs
      const canvas = this.$refs.myCanvas;
      const ctx = canvas.getContext("2d");

      // Create an image object
      const imgFirstCharacter = new Image();

      const firstPlaceChar = require("../assets/characters/sf6/Cammy.png");

      imgFirstCharacter.src = firstPlaceChar;

      imgFirstCharacter.onload = () => {
        ctx.drawImage(this.imgBackground, 0, 0, canvas.width, canvas.height);

        //Set another canvas for the masking
        const offscreenCanvas = document.createElement("canvas");
        offscreenCanvas.width = canvas.width;
        offscreenCanvas.height = canvas.height;
        const offscreenCtx = offscreenCanvas.getContext("2d");

        //Render chars
        offscreenCtx.drawImage(imgFirstCharacter, -170, 50, 841, 951);
        offscreenCtx.drawImage(imgFirstCharacter, -170 + 469, 50, 841, 951);
        offscreenCtx.drawImage(imgFirstCharacter, -170 + 940, 50, 841, 951);
        offscreenCtx.globalCompositeOperation = "destination-in";
        offscreenCtx.drawImage(this.imgMask, 0, 0, canvas.width, canvas.height);
        offscreenCtx.globalCompositeOperation = "source-over";

        ctx.drawImage(offscreenCanvas, 0, 0, canvas.width, canvas.height);
        ctx.drawImage(this.imgForeground, 0, 0, canvas.width, canvas.height);

        this.showSave = true;
      };
    },
    save() {
      const dataURL = this.$refs.myCanvas.toDataURL();

      // Create a link element
      const link = document.createElement("a");

      // Set the href attribute of the link to the data URL
      link.href = dataURL;

      // Specify the filename for the saved image
      link.download = "canvas_image.png";

      // Simulate a click on the link to trigger the download
      link.click();
    },
  },
};
</script>

<style scoped>
  .canvas-container {
    display: flex;
    flex-direction: column;
    padding-top: 30px;
    padding-left: 10px;
  }
  .el-button {
    max-width: 100px;
    margin-bottom: 10px;
  }
</style>