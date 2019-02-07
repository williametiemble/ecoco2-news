<template>
  <div id="app">

    <b-container fluid>
      <b-row>
        <b-col>
          <form>
            <select @change="chooseSource($event)">
              <option v-for="item in sources" :value="item.id" :key="item.id">
                {{ item.name }}
              </option>
            </select>
          </form> 
        </b-col>  
      </b-row> 
      <b-row>
        <b-col>
          <b-table responsive striped hover :items="articles" v-if="articles"></b-table>
        </b-col>
      </b-row>
    </b-container>

  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',
  data () {
    return {
      sources: null,
      articles: [],
      food: null
    }
  },
  methods: {
    chooseSource(event) {
      console.log(event.target.value)
      var _self = this
      axios
        .get('https://newsapi.org/v2/top-headlines?sources=bbc-news&apiKey=cd94713f6fe5467d893b99d5d69b75e5')
        .then(
          function (response) {
            response.data.articles.forEach(function(element) {
              _self.articles.push({
                title: element.title,
                image: element.urlToImage,
                content: element.content,
                url: element.url
              })
            });
          }
        )
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

</style>
