<template>
    <el-form :model="form" label-width="100px" label-position="left">
      <h2>Set Information</h2>
      <div style="width: 400px">
        <el-form-item label="Event">
          <el-select v-model="form.event" value-key="name">
            <el-option value="Smash Na Invicta" />
            <el-option value="Quebra Comandos" />
            <el-option value="Invicta Fighters" />
          </el-select>
        </el-form-item>
        <el-form-item label="Round">
          <el-input type="text" v-model="form.round" />
        </el-form-item>
        <el-form-item label="Date">
          <el-input type="text" v-model="form.date" />
        </el-form-item>
      </div>
      <el-form-item label="Game" v-show="form.event">
        <el-select v-model="form.game" @change="onGameSelect">
          <el-option
            v-if="
              form.event === 'Smash Na Invicta' ||
              form.event === 'Quebra Comandos'
            "
            class="select-option"
            label="Smash Ultimate"
            value="smash"
          >
            <img class="select-image" src="../assets/games/SSBU_Logo.png" />
          </el-option>
          <el-option
            v-if="form.event === 'Smash Na Invicta'"
            class="select-option"
            label="Rivals 2"
            value="roa2"
          >
            <img class="select-image" src="../assets/games/ROA2_Logo.png" />
          </el-option>
          <div v-if="form.event === 'Invicta Fighters'">
            <el-option class="select-option" label="Street Fighter 6" value="sf6">
              <img class="select-image" src="../assets/games/SF6_Logo.png" />
            </el-option>
            <el-option
              class="select-option"
              label="Granblue Fantasy Versus Rising"
              value="gbvsr"
            >
              <img class="select-image" src="../assets/games/GBVSR_Logo.png" />
            </el-option>
            <el-option
              class="select-option"
              label="Guilty Gear Strive"
              value="ggst"
            >
              <img class="select-image" src="../assets/games/GGST_Logo.png" />
            </el-option>
            <el-option
              class="select-option"
              label="Tekken 8"
              value="tekken8"
            >
              <img class="select-image" src="../assets/games/Tekken8_Logo.png" />
            </el-option>
            <el-option
              class="select-option"
              label="Under Night In-Birth Sys:Celes"
              value="uni2"
            >
              <img class="select-image" src="../assets/games/UNI2_Logo.png" />
            </el-option>
          </div>
        </el-select>
      </el-form-item>
      <div v-if="form.game">
        <h2>Player Data</h2>
        <div class="player-data">
          <div
            class="players"
            v-for="(player, name, index) in form.playerData"
            :key="player"
          >
            <p>Player {{ index + 1 }}</p>
            <el-form-item label="Name">
              <el-input v-model="player.name" />
            </el-form-item>
            <el-form-item label="Character">
              <el-select v-model="player.character" value-key="name" filterable>
                <el-option
                  v-for="character in currentCast"
                  :key="character.name"
                  :label="character.name"
                  :value="character"
                />
              </el-select>
            </el-form-item>
            <el-form-item v-if="form.game === 'smash'" label="Color">
              <el-select v-model="player.color">
                <el-option
                  v-for="index in 8" :key="index"
                  :label="`${index - 1}`"
                  :value="index - 1"
                /> 
              </el-select>
            </el-form-item>
            <el-form-item v-if="form.game === 'roa2'" label="Color">
              <el-select v-model="player.color">
                <el-option
                  v-for="index in 40" :key="index"
                  :label="`${index - 1}`"
                  :value="index - 1"
                /> 
              </el-select>
            </el-form-item>
            <el-form-item v-if="player.character" label="Override Data  ">
              <el-switch v-model="player.override"/>
            </el-form-item>
            <div v-if="player.override">
                <el-form-item v-if="player.character" label="Width">
                  <el-input v-model="player.character.renderDetails.width" />
                </el-form-item>
                 <el-form-item v-if="player.character" label="Height">
                  <el-input v-model="player.character.renderDetails.height" />
                 </el-form-item>
                 <el-form-item v-if="player.character" label="Pos X">
                  <el-input v-model="player.character.renderDetails.posX" />
                 </el-form-item>
                 <el-form-item v-if="player.character" label="Pos Y">
                  <el-input v-model="player.character.renderDetails.posY" />
                 </el-form-item>
              </div>
          </div>
        </div>
        <div class="generate-button-section">
          <el-button type="primary" @click="onClickGenerate">Generate</el-button>
        </div>
      </div>
    </el-form>
  </template>
  
  <script>
  import smashCharNames from "../characters/smash_char_list.json";
  import sf6CharNames from "../characters/sf6_char_list.json";
  import gbvsrCharNames from "../characters/gbvsr_char_list.json";
  import ggstCharNames from "../characters/ggst_char_list.json";
  import uni2CharNames from "../characters/uni2_char_list.json";
  import tekken8CharNames from "../characters/tekken8_char_list.json";
  import roa2CharNames from "../characters/roa2_char_list.json"
  
  export default {
    name: "Thumbnails",
    data() {
      return {
        form: {
          game: "",
          round: "",
          date: "",
          playerData: {
            first: { name: "", prefix: "", twitter: "", character: {}, color: '0', override: false },
            second: { name: "", prefix: "", twitter: "", character: {}, color: '0',override: false },
          },
        },
        currentCast: [],
        color: ""
      };
    },
    methods: {
      onGameSelect() {
        switch (this.form.game) {
          case "smash":
            this.currentCast = smashCharNames.characters;
            if(this.form.event == 'Smash Na Invicta')
              this.form.color = "#384695";
            else
              this.form.color = "#b71115";
            break;
          case "sf6":
            this.currentCast = sf6CharNames.characters;
            this.form.color = "#6d6d6d";
            break;
          case "gbvsr":
            this.currentCast = gbvsrCharNames.characters;
            this.form.color = "#058dd8";
            break;
          case "ggst":
            this.currentCast = ggstCharNames.characters;
            this.form.color = "#be262c";
            break;
          case "uni2":
            this.currentCast = uni2CharNames.characters;
            this.form.color = "#78008a";
            break;
          case "tekken8":
            this.currentCast = tekken8CharNames.characters;
            this.form.color = "#ce0d4e";
            break;
          case "roa2":
            this.currentCast = roa2CharNames.characters;
            this.form.color = "#384695";
            break;
          default:
            this.currentCast = [];
            break;
        }
      },
      onClickGenerate() {
        this.$emit("generate", this.form);
      },
    },
  };
  </script>
  
  <style scoped>
  .select-image {
    width: 100px;
  }
  .select-option {
    height: 100%;
  }
  h2 {
    text-align: left;
  }
  .player-data {
    display: flex;
  }
  
  .players {
    text-align: left;
    padding-top: 10px;
    padding-right: 50px;
    padding-left: 10px;
    border-style: solid;
  }
  
  .generate-button-section {
    float: left;
    padding-top: 30px;
  }
  </style>