<template>
  <div id="app">
    <VueMapper
      :debug="true"
      ApiKey="GOOGLE-API-KEY"
      id_override="MyCoolMap"
    />
  </div>
</template>

<script>
import VueMapper from './components/VueMapper.vue'

export default {
  name: 'app',
  components: {
    VueMapper
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
