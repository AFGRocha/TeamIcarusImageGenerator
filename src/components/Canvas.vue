<template>
  <canvas ref="myCanvas" width="1674" height="739"></canvas>
</template>

<script>
export default {
  name: "Canvas",
  data() {
    return {
    };
  },
  methods: {
    async teste() {
      // Get the canvas element and its context using $refs
      const canvas = this.$refs.myCanvas;
      const ctx = canvas.getContext("2d");

      // Create an image object
      const imgBackground = new Image();
      const imgForeground = new Image();
      const imgFirstCharacter = new Image();
      const imgMask = new Image();
      
      const background = require("../assets/backgrounds/smashnainvicta_background.png")
      const foreground = require("../assets/foregrounds/smashnainvicta_foreground.png")
      const firstPlaceChar = require("../assets/characters/sf6/Cammy.png")
      const mask = require("../assets/mask/smashnainvicta_mask.png")

      imgBackground.src =  background
     
      imgBackground.onload = await function () {
        ctx.drawImage(imgBackground, 0, 0, canvas.width, canvas.height);
      };

      imgFirstCharacter.src =  firstPlaceChar
     
      imgFirstCharacter.onload = await function () {
        ctx.drawImage(imgFirstCharacter, -170, 50, 841, 951);
      };
    

      imgForeground.src = foreground 

      imgForeground.onload = await function () {
        ctx.drawImage(imgForeground, 0, 0, canvas.width, canvas.height);
      };

      imgMask.src = mask 

      imgMask.onload = await function () {
        ctx.drawImage(imgMask, 0, 0, canvas.width, canvas.height);
      };

      const dataURL = canvas.toDataURL(); 

      // Create a link element
      const link = document.createElement("a");

      // Set the href attribute of the link to the data URL
      link.href = dataURL;

      // Specify the filename for the saved image
      link.download = "canvas_image.png"; 

      // Simulate a click on the link to trigger the download
      // link.click();
    },
  },
};
</script>

<style>
</style>