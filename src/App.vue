<template>
  <img class="team-icarus" src="./assets/Icarus1.png" />
  <el-tabs v-model="activeTab" class="demo-tabs" >
    <el-tab-pane label="Top 3" name="top3" />
    <el-tab-pane label="Thumbnails" name="thumbnails" />
  </el-tabs>
  <Top3 v-show="activeTab === 'top3'" @generate="handleGenerateTop3"/>
  <Thumbnails v-show="activeTab === 'thumbnails'" @generate="handleGenerateThumbnail"/>
  <Canvas ref="canvas" />

  <!-- Isto é bem parvo-->
  <div class="boldfont_loader">
    The Bold Font Loader
  </div>
  <div class="bebasneue_loader">
    Bebas Neue Loader asdsa
  </div>
</template>

<script>
import Top3 from "./components/Top3.vue";
import Canvas from "./components/Canvas.vue"
import Thumbnails from './components/Thumbnails.vue';

export default {
  name: "App",
  components: {
    Top3,
    Canvas,
    Thumbnails
  },
  data() {
    return {
      activeTab: "top3",
    };
  },
  methods: {
    handleGenerateTop3(form) {
      this.$refs.canvas.generateTop3(form);
    },
    handleGenerateThumbnail(form) {
      this.$refs.canvas.generateThumbnail(form);
    },
  },
};
</script>

<style>
body {
  background-color: #202124;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
}

.el-tabs__item {
  color: white;
}

.el-form-item__label {
  color: white !important;
}

.team-icarus {
  width: 70px;
  float: right;
}


@import url('https://fonts.cdnfonts.com/css/the-bold-font');

@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap');

.boldfont_loader {
  opacity: 0;
  font-family: 'The Bold Font';
}
.bebasneue_loader {
  opacity: 0;
  font-family: 'Bebas Neue';
}
</style>
