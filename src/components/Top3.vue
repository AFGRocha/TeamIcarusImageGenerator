<template>
  <el-form :model="form" label-width="100px" label-position="left">
    <el-form-item label="Game">
      <el-select v-model="form.game">
        <el-option class="select-option" label="Smash Ultimate" value="smash">
          <img class="select-image" src="../assets/games/SSBU_Logo.png">
        </el-option>
        <el-option class="select-option" label="Street Fighter 6" value="sf6">
          <img class="select-image" src="../assets/games/SF6_Logo.png">
        </el-option>
        <el-option class="select-option" label="Granblue Fantasy Versus Rising" value="gbvsr">
          <img class="select-image" src="../assets/games/GBVSR_Logo.png">
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="Venue">
      <el-select v-model="form.venue">
        <el-option class="select-option" label="Aniplay" value="aniplay">
          <img class="select-image" src="../assets/venues/aniplay.png">
        </el-option>
        <el-option class="select-option" label="Quebra-Dados" value="qd">
          <img class="select-image" src="../assets/venues/qd_black.png">
        </el-option>
      </el-select>
    </el-form-item>
    <div style="width: 400px">
      <el-form-item label="Event name">
        <el-input v-model="form.name" />
      </el-form-item>
      <el-form-item label="Start.gg page">
        <el-input v-model="form.page" />
      </el-form-item>
      <el-form-item label="Entrants">
        <el-input v-model="form.entrants" />
      </el-form-item>
      <el-form-item label="Date">
        <el-input v-model="form.date" />
      </el-form-item>
    </div>
  </el-form>
</template>

<script>
export default {
  name: 'Top3',
  data() {
    return {
      form:  {
        game: '',
        venue: '',
        name: '',
        page: '',
        entrants: '',
        date: ''
      }
    }
  },
}
</script>

<style scoped>
  .select-image {
    width: 100px;
  }
  .select-option {
    height: 100%;
  }
  el-input {
    width: 200px;
  }
</style>
