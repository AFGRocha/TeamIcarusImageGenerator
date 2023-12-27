<template>
  <el-tabs v-model="activeTab" class="demo-tabs" @tab-click="handleClick">
    <el-tab-pane label="Top 3" name="top3"/>
    <el-tab-pane label="Thumbnails" name="thumbnails"/>
  </el-tabs>
  <Top3 v-show="activeTab ==='top3'"/>
</template>

<script>
import Top3 from './components/Top3.vue'

export default {
  name: 'App',
  components: {
    Top3
  },
  data() {
    return {
      activeTab: 'top3'
    }
  },
  methods: {
    handleClick() {
      console.log(this.activeTab)
    }
  }
}
</script>

<style>
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
</style>
