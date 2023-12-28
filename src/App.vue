<template>
  <img class="team-icarus" src="./assets/Icarus1.png" />
  <el-tabs v-model="activeTab" class="demo-tabs" >
    <el-tab-pane label="Top 3" name="top3" />
    <el-tab-pane label="Thumbnails" name="thumbnails" />
  </el-tabs>
  <Top3 v-show="activeTab === 'top3'" @generate="handleGenerateClick"/>
  <Canvas ref="canvas" />
</template>

<script>
import Top3 from "./components/Top3.vue";
import Canvas from "./components/Canvas.vue"

export default {
  name: "App",
  components: {
    Top3,
    Canvas
  },
  data() {
    return {
      activeTab: "top3",
    };
  },
  methods: {
    handleGenerateClick() {
      this.$refs.canvas.teste();
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
</style>
