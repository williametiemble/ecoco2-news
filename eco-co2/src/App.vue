<template>
  <div id="app">
    <form>
      <select>
        <option v-for="item in sources" :value="item" :key="item.id">
          {{ item.name }}
        </option>
      </select>
    </form>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',
  data () {
    return {
      sources: null,

    }
  },
  mounted () {
    axios
      .get('https://newsapi.org/v2/sources?apiKey=cd94713f6fe5467d893b99d5d69b75e5')
      .then(
        response => (this.sources = response.data.sources )
      )
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
