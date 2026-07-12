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
      firstCharMask: new Image(),
      secondCharMask: new Image(),
      thirdCharMask: new Image(),
      p1ThumbnailMask: new Image(),
      p2ThumbnailMask: new Image(),
      aniplayLogo: new Image(),
      qdLogo: new Image(),
      drawstepLogo: new Image(),
      twitterIcon: new Image(),
      showSave: false,
    };
  },
  mounted() {
    this.firstCharMask.src = require("../assets/mask/mask1.png");
    this.secondCharMask.src = require("../assets/mask/mask2.png");
    this.thirdCharMask.src = require("../assets/mask/mask3.png");
    this.p1ThumbnailMask.src = require("../assets/mask/maskThumbnailp1.png");
    this.p2ThumbnailMask.src = require("../assets/mask/maskThumbnailp2.png");
    this.aniplayLogo.src = require("../assets/venues/aniplay.png");
    this.qdLogo.src = require("../assets/venues/qd.png");
    this.drawstepLogo.src = require("../assets/venues/drawstep.png");
    this.twitterIcon.src =  require("../assets/twitter_icon.png");
  },
  methods: {
    async generateTop3(form) {
      // Get the canvas element and its context using $refs
      const canvas = this.$refs.myCanvas;
      canvas.width = 1674
      canvas.height = 739
      const ctx = canvas.getContext("2d");

      this.imgBackground.src = require(`../assets/backgrounds/${form.event}_${form.game}_background.png`);
      this.imgForeground.src = require(`../assets/foregrounds/${form.event}_${form.game}_foreground.png`);

      // Create an image object
      const imgFirstCharacter = new Image();
      const imgSecondCharacter = new Image();
      const imgThirdCharacter = new Image();

      //2xko
      const imgFirstCharacter2 = new Image();
      const imgSecondCharacter2 = new Image();
      const imgThirdCharacter2 = new Image();

      let firstPlaceChar
      let secondPlaceChar
      let thirdPlaceChar

      // 2xko
      let firstPlaceChar2 = null
      let secondPlaceChar2 = null
      let thirdPlaceChar2 = null

      if (form.game === 'smash') {
        firstPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.first.character.name}_0${form.playerData.first.color}.png`);
        secondPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.second.character.name}_0${form.playerData.second.color}.png`);
        thirdPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.third.character.name}_0${form.playerData.third.color}.png`);
      } else if (form.game === 'roa2') {
        firstPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.first.character.name}_${form.playerData.first.color}.png`);
        secondPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.second.character.name}_${form.playerData.second.color}.png`);
        thirdPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.third.character.name}_${form.playerData.third.color}.png`);
      } else if (form.game === '2xko') {
        firstPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.first.character.name}.png`);
        firstPlaceChar2 = require(`../assets/characters/${form.game}/${form.playerData.first.character2.name}.png`);
        secondPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.second.character.name}.png`);
        secondPlaceChar2 = require(`../assets/characters/${form.game}/${form.playerData.second.character2.name}.png`);
        thirdPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.third.character.name}.png`);
        thirdPlaceChar2 = require(`../assets/characters/${form.game}/${form.playerData.third.character2.name}.png`);
      } else {
        firstPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.first.character.name}.png`);
        secondPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.second.character.name}.png`);
        thirdPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.third.character.name}.png`);
      }

      imgFirstCharacter.src = firstPlaceChar;
      imgSecondCharacter.src = secondPlaceChar;
      imgThirdCharacter.src = thirdPlaceChar;

      //2xko
      imgFirstCharacter2.src = firstPlaceChar2;
      imgSecondCharacter2.src = secondPlaceChar2;
      imgThirdCharacter2.src = thirdPlaceChar2;

      const imagesToLoad = form.game !== '2xko' ? [this.imgBackground, this.imgForeground, imgFirstCharacter, imgSecondCharacter, imgThirdCharacter] : [this.imgBackground, this.imgForeground, imgFirstCharacter, imgSecondCharacter, imgThirdCharacter, imgFirstCharacter2, imgSecondCharacter2, imgThirdCharacter2];
      let loadedCounter = 0;
      await document.fonts.ready;
      for (let i = 0; i < imagesToLoad.length; i++) { 
        imagesToLoad[i].onload = () => {
          loadedCounter++
          if(loadedCounter == imagesToLoad.length) {
            ctx.drawImage(this.imgBackground, 0, 0, canvas.width, canvas.height);

            //Mask first place character
            const char1Canvas = this.maskCharacter(imgFirstCharacter, this.firstCharMask, form.playerData.first.character.renderDetails, 0, form.game, imgFirstCharacter2, form.playerData.first.character2.renderDetails)
            const char2Canvas = this.maskCharacter(imgSecondCharacter, this.secondCharMask, form.playerData.second.character.renderDetails, 469, form.game, imgSecondCharacter2, form.playerData.second.character2.renderDetails)
            const char3Canvas = this.maskCharacter(imgThirdCharacter, this.thirdCharMask, form.playerData.third.character.renderDetails, 940, form.game, imgThirdCharacter2, form.playerData.third.character2.renderDetails)


            ctx.drawImage(char1Canvas, 0, 0, canvas.width, canvas.height);
            ctx.drawImage(char2Canvas, 0, 0, canvas.width, canvas.height);
            ctx.drawImage(char3Canvas, 0, 0, canvas.width, canvas.height);
            ctx.drawImage(this.imgForeground, 0, 0, canvas.width, canvas.height);

            //Text
            ctx.textAlign = "center";
            
            //First Place name and prefix
            let firstFontSize = 96;
            ctx.font = `${firstFontSize}px "Bebas Neue"`
            let firstPrefixWidth = ctx.measureText(form.playerData.first.prefix).width;
            let firstNameWidth = ctx.measureText(form.playerData.first.name).width;
            while ((firstPrefixWidth + firstNameWidth) > 410 && firstFontSize > 20) {
              firstFontSize -= 2;
              ctx.font = `${firstFontSize}px "Bebas Neue"`
              firstPrefixWidth = ctx.measureText(form.playerData.first.prefix).width;
              firstNameWidth = ctx.measureText(form.playerData.first.name).width;
            }
            ctx.fillStyle = "#fbd059";
            ctx.fillText(form.playerData.first.prefix, 245 - firstNameWidth / 2, 625);
            ctx.fillStyle = "#FFFFFF";
            const firstNameX = ((form.playerData.first.prefix) ? 260 : 250);
            ctx.fillText(form.playerData.first.name, firstNameX + firstPrefixWidth / 2, 625)  
            //First Place twitter
            if(form.playerData.first.twitter) {
              let twitterFontSize = 42;
              ctx.font = `${twitterFontSize}px "Bebas Neue"`
              let twitterWidth = ctx.measureText(`@${form.playerData.first.twitter}`).width;
              while (twitterWidth > 410 && twitterFontSize > 20) {
                twitterFontSize -= 2;
                ctx.font = `${twitterFontSize}px "Bebas Neue"`
                twitterWidth = ctx.measureText(`@${form.playerData.first.twitter}`).width;
              }
              ctx.fillStyle = "#000000";
              ctx.fillText(`@${form.playerData.first.twitter}`, 245, 677);
              ctx.drawImage(this.twitterIcon, 45, 645);
            }

            //Second Place name and prefix
            let secondFontSize = 96;
            ctx.font = `${secondFontSize}px "Bebas Neue"`
            let secondPrefixWidth = ctx.measureText(form.playerData.second.prefix).width;
            let secondNameWidth = ctx.measureText(form.playerData.second.name).width;
            while ((secondPrefixWidth + secondNameWidth) > 410 && secondFontSize > 20) {
              secondFontSize -= 2;
              ctx.font = `${secondFontSize}px "Bebas Neue"`
              secondPrefixWidth = ctx.measureText(form.playerData.second.prefix).width;
              secondNameWidth = ctx.measureText(form.playerData.second.name).width;
            }
            ctx.fillStyle = "#fbd059";
            ctx.fillText(form.playerData.second.prefix, 715 - secondNameWidth / 2, 625);
            ctx.fillStyle = "#FFFFFF";
            const secondNameX = ((form.playerData.second.prefix) ? 730 : 720);
            ctx.fillText(form.playerData.second.name, secondNameX + secondPrefixWidth / 2, 625)  
            //Second Place twitter
            if(form.playerData.second.twitter) {
              let twitterFontSize = 42;
              ctx.font = `${twitterFontSize}px "Bebas Neue"`
              let twitterWidth = ctx.measureText(`@${form.playerData.second.twitter}`).width;
              while (twitterWidth > 410 && twitterFontSize > 20) {
                twitterFontSize -= 2;
                ctx.font = `${twitterFontSize}px "Bebas Neue"`
                twitterWidth = ctx.measureText(`@${form.playerData.second.twitter}`).width;
              }
              ctx.fillStyle = "#000000";
              ctx.fillText(`@${form.playerData.second.twitter}`, 715, 682);
              ctx.drawImage(this.twitterIcon, 515, 645);
            }

            //Third Place name and prefix
            let thirdFontSize = 96;
            ctx.font = `${thirdFontSize}px "Bebas Neue"`
            let thirdPrefixWidth = ctx.measureText(form.playerData.third.prefix).width;
            let thirdNameWidth = ctx.measureText(form.playerData.third.name).width;
            while ((thirdPrefixWidth + thirdNameWidth) > 410 && thirdFontSize > 20) {
              thirdFontSize -= 2;
              ctx.font = `${thirdFontSize}px "Bebas Neue"`
              thirdPrefixWidth = ctx.measureText(form.playerData.third.prefix).width;
              thirdNameWidth = ctx.measureText(form.playerData.third.name).width;
            }
            ctx.fillStyle = "#fbd059";
            ctx.fillText(form.playerData.third.prefix, 1187 - thirdNameWidth / 2, 625);
            ctx.fillStyle = "#FFFFFF";
            const thirdNameX = ((form.playerData.third.prefix) ? 1202 : 1187);
            ctx.fillText(form.playerData.third.name, thirdNameX + thirdPrefixWidth / 2, 625)  
            //Third Place twitter
            if(form.playerData.third.twitter) {
              let twitterFontSize = 42;
              ctx.font = `${twitterFontSize}px "Bebas Neue"`
              let twitterWidth = ctx.measureText(`@${form.playerData.third.twitter}`).width;
              while (twitterWidth > 410 && twitterFontSize > 20) {
                twitterFontSize -= 2;
                ctx.font = `${twitterFontSize}px "Bebas Neue"`
                twitterWidth = ctx.measureText(`@${form.playerData.third.twitter}`).width;
              }
              ctx.fillStyle = "#000000";
              ctx.fillText(`@${form.playerData.third.twitter}`, 1187, 677);
              ctx.drawImage(this.twitterIcon, 987, 645);
            }

            //Tournament name with number
            ctx.font = form.event == "Quebra Comandos" && form.edition >= 100 ? '18px "The Bold Font"' : '20px "The Bold Font"'
            ctx.fillStyle = "#FFFFFF";
            ctx.fillText(`${form.event} #${form.edition}`, 1557, 235);

            //Start.gg
            ctx.font = 
            ctx.font = form.event == "Quebra Comandos" && form.edition >= 100 ? '20px "Bebas Neue"' : '22px "Bebas Neue"'
            ctx.fillStyle = "#FFFFFF";
            ctx.fillText(`start.gg/`, 1557 - ctx.measureText(form.page).width / 2, 634);
            ctx.fillStyle = form.color;
            ctx.fillText(form.page, 1557 + ctx.measureText(`start.gg/`).width / 2, 634) 

            //Entrants
            ctx.fillText(form.entrants, 1557 - ctx.measureText(' Entrants').width / 2, 679);
            ctx.fillStyle = "#FFFFFF";
            ctx.fillText(' Entrants', 1557 + ctx.measureText(form.entrants).width / 2, 679) 

            //Date  
            // ctx.fillStyle = "#FFFFFF"; 
            ctx.fillText(form.date + ' - ', 1557 - ctx.measureText('Porto').width / 2, 726);
            ctx.fillStyle = form.color
            ctx.fillText('Porto', 1557 + ctx.measureText(form.date + ' - ').width / 2, 726) 


            //Venue logo
            if(form.venue === 'drawstep') {
              ctx.drawImage(this[form.venue + 'Logo'], 1495, 490, 120, 119);

            } else {
              ctx.drawImage(this[form.venue + 'Logo'], 1433, 490);
            }
            this.showSave = true;
          }
        }
      }
    
    },
    async generateThumbnail(form) {
      // Get the canvas element and its context using $refs
      const canvas = this.$refs.myCanvas;
      canvas.width = 1280
      canvas.height = 720
      const ctx = canvas.getContext("2d");

      this.imgBackground.src = require(`../assets/thumbnails/backgrounds/default.png`);
      if(form.game === 'smash' || form.game === 'roa2') {
        this.imgForeground.src = require(`../assets/thumbnails/foregrounds/${form.event}_foreground.png`);
      } else {
        this.imgForeground.src = require(`../assets/thumbnails/foregrounds/${form.event}_${form.game}_foreground.png`);
      }
      
      

      let player1Char = new Image();
      let player2Char = new Image();
      if (form.game === 'smash') {
        player1Char.src = require(`../assets/characters/${form.game}/${form.playerData.first.character.name}_0${form.playerData.first.color}.png`);
        player2Char.src = require(`../assets/characters/${form.game}/${form.playerData.second.character.name}_0${form.playerData.second.color}.png`);
      } else if (form.game === 'roa2') {
        player1Char.src = require(`../assets/characters/${form.game}/${form.playerData.first.character.name}_${form.playerData.first.color}.png`);
        player2Char.src = require(`../assets/characters/${form.game}/${form.playerData.second.character.name}_${form.playerData.second.color}.png`);
      } else {
        player1Char.src = require(`../assets/characters/${form.game}/${form.playerData.first.character.name}.png`);
        player2Char.src = require(`../assets/characters/${form.game}/${form.playerData.second.character.name}.png`);
      }

      const imagesToLoad = [this.imgBackground, this.imgForeground, player1Char, player2Char]
      let loadedCounter = 0;
      for (let i = 0; i < imagesToLoad.length; i++) {
        imagesToLoad[i].onload = () => {
          loadedCounter++
          if(loadedCounter == imagesToLoad.length) {
            ctx.drawImage(this.imgBackground, 0, 0, canvas.width, canvas.height);
            let player1 = this.thumbnailMask(player1Char, this.p1ThumbnailMask, form.playerData.first.character.renderDetails, -200, form.game)
            ctx.drawImage(player1, 0, 0, canvas.width, canvas.height);
            let player2 = this.thumbnailMask(player2Char, this.p2ThumbnailMask, form.playerData.second.character.renderDetails, 500, form.game)
            ctx.drawImage(player2, 0, 0, canvas.width, canvas.height);
            ctx.drawImage(this.imgForeground, 0, 0, canvas.width, canvas.height);

            

            const textCanvas = document.createElement("canvas");
            textCanvas.width = this.$refs.myCanvas.width;
            textCanvas.height = this.$refs.myCanvas.height;
            const textCanvasCtx = textCanvas.getContext("2d");
            textCanvasCtx.rotate(-2 * Math.PI / 180);
            textCanvasCtx.textAlign = "center";
            //P1 Name
            textCanvasCtx.font = 'italic 105px "Bebas Neue"'
            textCanvasCtx.fillStyle = "rgba(0, 0, 0, 0.5)";
            textCanvasCtx.fillText(form.playerData.first.name, 305, 110);
            textCanvasCtx.fillStyle = "#FFFFFF";
            textCanvasCtx.fillText(form.playerData.first.name, 300, 105);

            //P2 Name
            textCanvasCtx.font = 'italic 105px "Bebas Neue"'
            textCanvasCtx.fillStyle = "rgba(0, 0, 0, 0.5)";
            textCanvasCtx.fillText(form.playerData.second.name, 965, 130);
            textCanvasCtx.fillStyle = "#FFFFFF";
            textCanvasCtx.fillText(form.playerData.second.name, 960, 125);

            //Round
            if(textCanvasCtx.measureText(form.round).width > 640) {
              textCanvasCtx.font = 'italic 80px "Bebas Neue"'
              textCanvasCtx.fillStyle = "rgba(0, 0, 0, 0.5)";
              textCanvasCtx.fillText(form.round, 290, 710);
              textCanvasCtx.fillStyle = "#FFFFFF";
              textCanvasCtx.fillText(form.round, 285, 705);
            } else {
              textCanvasCtx.font = 'italic 105px "Bebas Neue"'
              textCanvasCtx.fillStyle = "rgba(0, 0, 0, 0.5)";
              textCanvasCtx.fillText(form.round, 290, 720);
              textCanvasCtx.fillStyle = "#FFFFFF";
              textCanvasCtx.fillText(form.round, 285, 715);
            }

            //Date
            textCanvasCtx.font = 'italic 105px "Bebas Neue"'
            textCanvasCtx.fillStyle = "rgba(0, 0, 0, 0.5)";
            textCanvasCtx.fillText(form.date, 965, 735);
            textCanvasCtx.fillStyle = "#FFFFFF";
            textCanvasCtx.fillText(form.date, 960, 730);

            ctx.drawImage(textCanvas,0,0, canvas.width, canvas.height)
          }
        }
      }

      this.showSave = true;

    },
    save() {
      const dataURL = this.$refs.myCanvas.toDataURL();

      // Create a link element
      const link = document.createElement("a");

      // Set the href attribute of the link to the data URL
      link.href = dataURL;

      // Specify the filename for the saved image
      link.download = "generated_TI_image.png";

      // Simulate a click on the link to trigger the download
      link.click();
    }, 
    maskCharacter(image, mask, renderDetails, xModifier, game, secondImage2xko = null, secondRenderDetails2xko = null) {
      const offscreenCanvas = document.createElement("canvas");
      offscreenCanvas.width = this.$refs.myCanvas.width;
      offscreenCanvas.height = this.$refs.myCanvas.height;
      const offscreenCtx = offscreenCanvas.getContext("2d");
      
      if (game === '2xko' && secondImage2xko) {
        const combinedCanvas = document.createElement("canvas");
        combinedCanvas.width = this.$refs.myCanvas.width;
        combinedCanvas.height = this.$refs.myCanvas.height;
        const combinedCtx = combinedCanvas.getContext("2d");

        combinedCtx.drawImage(secondImage2xko, parseInt(secondRenderDetails2xko.posX) + xModifier + 80, parseInt(secondRenderDetails2xko.posY) - 100, secondRenderDetails2xko.width, secondRenderDetails2xko.height);
        combinedCtx.drawImage(image, parseInt(renderDetails.posX) + xModifier - 70, parseInt(renderDetails.posY) + 100, renderDetails.width, renderDetails.height);
        
        offscreenCtx.drawImage(combinedCanvas, 0, 0);
        offscreenCtx.globalCompositeOperation = "source-in";
        offscreenCtx.drawImage(mask, 0, 0, this.$refs.myCanvas.width, this.$refs.myCanvas.height);
        offscreenCtx.globalCompositeOperation = "source-over";

        offscreenCtx.drawImage(combinedCanvas, 20, 10);
        offscreenCtx.globalCompositeOperation = "destination-in";
        offscreenCtx.drawImage(mask, 0, 0, this.$refs.myCanvas.width, this.$refs.myCanvas.height);
        offscreenCtx.globalCompositeOperation = "source-over";
      } else {
        if(game != 'sf6' && game != 'cotw') {
          offscreenCtx.drawImage(image, parseInt(renderDetails.posX) + xModifier + 20, parseInt(renderDetails.posY) + 10, renderDetails.width, renderDetails.height);
          offscreenCtx.globalCompositeOperation = "source-in";
          offscreenCtx.drawImage(mask, 0, 0, this.$refs.myCanvas.width, this.$refs.myCanvas.height);
          offscreenCtx.globalCompositeOperation = "source-over";
        }
        
        offscreenCtx.drawImage(image, parseInt(renderDetails.posX)  + xModifier, renderDetails.posY, renderDetails.width, renderDetails.height);
        offscreenCtx.globalCompositeOperation = "destination-in";
        offscreenCtx.drawImage(mask, 0, 0, this.$refs.myCanvas.width, this.$refs.myCanvas.height);
        offscreenCtx.globalCompositeOperation = "source-over";
      }

      return offscreenCanvas
    },
    thumbnailMask(image, mask, renderDetails, xModifier, game) {
      let scaleModifier = 450;
      let posModifierX = 0;
      let posModifierY = 0;

      if(game !== 'smash' && game !== 'roa2') {
        scaleModifier = 0
        posModifierX = 250;
        posModifierY = 100;
      }
        

      const offscreenCanvas = document.createElement("canvas");
      offscreenCanvas.width = this.$refs.myCanvas.width;
      offscreenCanvas.height = this.$refs.myCanvas.height;
      const offscreenCtx = offscreenCanvas.getContext("2d");

      offscreenCtx.drawImage(image, parseInt(renderDetails.posX) + xModifier + posModifierX, parseInt(renderDetails.posY) - 50 + posModifierY, renderDetails.width + scaleModifier, renderDetails.height + scaleModifier);
      offscreenCtx.globalCompositeOperation = "destination-in";
      offscreenCtx.drawImage(mask, 0, 0, this.$refs.myCanvas.width, this.$refs.myCanvas.height);
      offscreenCtx.globalCompositeOperation = "source-over";
      return offscreenCanvas
    }
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