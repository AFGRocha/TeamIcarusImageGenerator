<template>
  <el-form :model="form" label-width="100px" label-position="left">
    <h2>Tournament Information</h2>
    <div style="width: 400px">
      <el-form-item label="Event">
        <el-select v-model="form.event" value-key="name">
          <el-option value="Smash Na Invicta" />
          <el-option value="Quebra Comandos" />
          <el-option value="Invicta Fighters" />
        </el-select>
      </el-form-item>
      <el-form-item label="Edition">
        <el-input type="number" v-model="form.edition" />
      </el-form-item>
      <el-form-item label="Start.gg page">
        <el-input v-model="form.page" />
      </el-form-item>
      <el-form-item label="Entrants">
        <el-input type="number" v-model="form.entrants" />
      </el-form-item>
      <el-form-item label="Date">
        <el-input v-model="form.date" />
      </el-form-item>
    </div>
    <el-form-item label="Venue">
      <el-select v-model="form.venue">
        <el-option class="select-option" label="Aniplay" value="aniplay">
          <img class="select-image" src="../assets/venues/aniplay.png" />
        </el-option>
        <el-option class="select-option" label="Quebra-Dados" value="qd">
          <img class="select-image" src="../assets/venues/qd_black.png" />
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="Game">
      <el-select v-model="form.game" @change="onGameSelect">
        <el-option class="select-option" label="Smash Ultimate" value="smash">
          <img class="select-image" src="../assets/games/SSBU_Logo.png" />
        </el-option>
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
          <el-form-item label="Prefix">
            <el-input v-model="player.prefix" />
          </el-form-item>
          <el-form-item label="Name">
            <el-input v-model="player.name" />
          </el-form-item>
          <el-form-item label="Twitter">
            <el-input v-model="player.twitter" />
          </el-form-item>
          <el-form-item label="Character">
            <el-select v-model="player.character" value-key="name">
              <el-option
                v-for="character in currentCast"
                :key="character.name"
                :label="character.name"
                :value="character"
              />
            </el-select>
          </el-form-item>
          <el-form-item v-if="form.game === 'smash'" label="Color">
            <el-select v-model="player.character" value-key="name">
              <el-option
                v-for="character in currentCast"
                :key="character.name"
                :label="character.name"
                :value="character"
              />
            </el-select>
          </el-form-item>
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

export default {
  name: "Top3",
  data() {
    return {
      form: {
        game: "",
        venue: "",
        event: "",
        edition: "",
        page: "",
        entrants: "",
        date: "",
        playerData: {
          first: { name: "", prefix: "", twitter: "", character: {} },
          second: { name: "", prefix: "", twitter: "", character: {} },
          third: { name: "", prefix: "", twitter: "", character: {} },
        },
      },
      currentCast: [],
    };
  },
  methods: {
    onGameSelect() {
      switch (this.form.game) {
        case "smash":
          this.currentCast = smashCharNames.characters;
          break;
        case "sf6":
          this.currentCast = sf6CharNames.characters;
          break;
        case "gbvsr":
          this.currentCast = gbvsrCharNames.characters;
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