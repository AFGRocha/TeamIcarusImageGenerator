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
      aniplayLogo: new Image(),
      qdLogo: new Image(),
      twitterIcon: new Image(),
      showSave: false,
    };
  },
  mounted() {
    this.firstCharMask.src = require("../assets/mask/mask1.png");
    this.secondCharMask.src = require("../assets/mask/mask2.png");
    this.thirdCharMask.src = require("../assets/mask/mask3.png");
    this.aniplayLogo.src = require("../assets/venues/aniplay.png");
    this.qdLogo.src = require("../assets/venues/qd.png");
    this.twitterIcon.src =  require("../assets/twitter_icon.png");
  },
  methods: {
    async generate(form) {
      console.log(form)
      // Get the canvas element and its context using $refs
      const canvas = this.$refs.myCanvas;
      const ctx = canvas.getContext("2d");

      this.imgBackground.src = require(`../assets/backgrounds/${form.event}_${form.game}_background.png`);
      this.imgForeground.src = require(`../assets/foregrounds/${form.event}_${form.game}_foreground.png`);

      // Create an image object
      const imgFirstCharacter = new Image();
      const imgSecondCharacter = new Image();
      const imgThirdCharacter = new Image();
      let firstPlaceChar
      let secondPlaceChar
      let thirdPlaceChar
      if (form.game === 'smash') {
        firstPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.first.character.name}_0${form.playerData.first.color}.png`);
        secondPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.second.character.name}_0${form.playerData.second.color}.png`);
        thirdPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.third.character.name}_0${form.playerData.third.color}.png`);
      } 
      else {
        firstPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.first.character.name}.png`);
        secondPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.second.character.name}.png`);
        thirdPlaceChar = require(`../assets/characters/${form.game}/${form.playerData.third.character.name}.png`);
      }

      imgFirstCharacter.src = firstPlaceChar;
      imgSecondCharacter.src = secondPlaceChar;
      imgThirdCharacter.src = thirdPlaceChar;
      
      const imagesToLoad = [this.imgBackground, this.imgForeground, imgFirstCharacter, imgSecondCharacter, imgThirdCharacter]
      let loadedCounter = 0;
      await document.fonts.ready;
      for (let i = 0; i < imagesToLoad.length; i++) {
        imagesToLoad[i].onload = () => {
          loadedCounter++
          if(loadedCounter == imagesToLoad.length) {
            ctx.drawImage(this.imgBackground, 0, 0, canvas.width, canvas.height);

            //Mask first place character
            const char1Canvas = this.maskCharacter(imgFirstCharacter, this.firstCharMask, form.playerData.first.character.renderDetails, 0, form.game)
            const char2Canvas = this.maskCharacter(imgSecondCharacter, this.secondCharMask, form.playerData.second.character.renderDetails, 469, form.game)
            const char3Canvas = this.maskCharacter(imgThirdCharacter, this.thirdCharMask, form.playerData.third.character.renderDetails, 940, form.game)

            ctx.drawImage(char1Canvas, 0, 0, canvas.width, canvas.height);
            ctx.drawImage(char2Canvas, 0, 0, canvas.width, canvas.height);
            ctx.drawImage(char3Canvas, 0, 0, canvas.width, canvas.height);
            ctx.drawImage(this.imgForeground, 0, 0, canvas.width, canvas.height);

            //Text
            ctx.textAlign = "center";
            
            //First Place name and prefix
            ctx.font = '96px "Bebas Neue"'
            ctx.fillStyle = "#fbd059";
            ctx.fillText(form.playerData.first.prefix, 245 - ctx.measureText(form.playerData.first.name).width / 2, 625);
            ctx.fillStyle = "#FFFFFF";
            const firstNameX = ((form.playerData.first.prefix) ? 260 : 245);
            ctx.fillText(form.playerData.first.name, firstNameX + ctx.measureText(form.playerData.first.prefix).width / 2, 625)  
            //First Place twitter
            if(form.playerData.first.twitter) {
              ctx.font = '42px "Bebas Neue"'
              ctx.fillStyle = "#000000";
              ctx.fillText(`@${form.playerData.first.twitter}`, 245, 677);
              ctx.drawImage(this.twitterIcon, 45, 645);
            }

            //Second Place name and prefix
            ctx.font = '96px "Bebas Neue"'
            ctx.fillStyle = "#fbd059";
            ctx.fillText(form.playerData.second.prefix, 715 - ctx.measureText(form.playerData.second.name).width / 2, 625);
            ctx.fillStyle = "#FFFFFF";
            const secondNameX = ((form.playerData.second.prefix) ? 730 : 715);
            ctx.fillText(form.playerData.second.name, secondNameX + ctx.measureText(form.playerData.second.prefix).width / 2, 625)  
            //Second Place twitter
            if(form.playerData.second.twitter) {
              ctx.font = '42px "Bebas Neue"'
              ctx.fillStyle = "#000000";
              ctx.fillText(`@${form.playerData.second.twitter}`, 715, 677);
              ctx.drawImage(this.twitterIcon, 515, 645);
            }

            //Third Place name and prefix
            ctx.font = '96px "Bebas Neue"'
            ctx.fillStyle = "#fbd059";
            ctx.fillText(form.playerData.third.prefix, 1187 - ctx.measureText(form.playerData.third.name).width / 2, 625);
            ctx.fillStyle = "#FFFFFF";
            const thirdNameX = ((form.playerData.third.prefix) ? 1202 : 1187);
            ctx.fillText(form.playerData.third.name, thirdNameX + ctx.measureText(form.playerData.third.prefix).width / 2, 625)  
            //Third Place twitter
            if(form.playerData.third.twitter) {
              ctx.font = '42px "Bebas Neue"'
              ctx.fillStyle = "#000000";
              ctx.fillText(`@${form.playerData.third.twitter}`, 1187, 677);
              ctx.drawImage(this.twitterIcon, 987, 645);
            }

            //Tournament name with number
            ctx.font = '20px "The Bold Font"'
            ctx.fillStyle = "#FFFFFF";
            ctx.fillText(`${form.event} #${form.edition}`, 1557, 235);

            //Start.gg
            ctx.font = '22px "Bebas Neue"'
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
            ctx.drawImage(this[form.venue + 'Logo'], 1433, 490);
            this.showSave = true;
          }
        }
      }
    
    },
    save() {
      const dataURL = this.$refs.myCanvas.toDataURL();

      // Create a link element
      const link = document.createElement("a");

      // Set the href attribute of the link to the data URL
      link.href = dataURL;

      // Specify the filename for the saved image
      link.download = "generated_top3.png";

      // Simulate a click on the link to trigger the download
      link.click();
    }, 
    maskCharacter(image, mask, renderDetails, xModifier, game) {
      const offscreenCanvas = document.createElement("canvas");
      offscreenCanvas.width = this.$refs.myCanvas.width;
      offscreenCanvas.height = this.$refs.myCanvas.height;
      const offscreenCtx = offscreenCanvas.getContext("2d");
      
      if(game != 'sf6') {
        console.log('entra');
        offscreenCtx.drawImage(image, parseInt(renderDetails.posX) + xModifier + 20, parseInt(renderDetails.posY) + 10, renderDetails.width, renderDetails.height);
        offscreenCtx.globalCompositeOperation = "source-in";
        offscreenCtx.drawImage(mask, 0, 0, this.$refs.myCanvas.width, this.$refs.myCanvas.height);
        offscreenCtx.globalCompositeOperation = "source-over";
      }
      

      offscreenCtx.drawImage(image, parseInt(renderDetails.posX)  + xModifier, renderDetails.posY, renderDetails.width, renderDetails.height);
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