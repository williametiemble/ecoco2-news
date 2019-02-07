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
        <b-col cols="12" md="3"v-for="article in articles">
          <b-card :title="article.title"
                  :img-src="article.image"
                  :img-alt="article.title"
                  img-top
                  class="mb-2">
            <p class="card-text">
              {{article.content}}
            </p>
            <b-button :href="article.url" variant="primary">Voir la news</b-button>
          </b-card>        
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
    var _self = this
    axios
      .get('https://newsapi.org/v2/sources?apiKey=cd94713f6fe5467d893b99d5d69b75e5')
      .then(
        response => (this.sources = response.data.sources )
          /*function (response) {
            response.data.articles.forEach(function(element) {
              _self.sources.push({
                title: element.title,
                image: element.urlToImage,
                content: element.content,
                url: element.url
              })
            });
          } 
          */       
      )
  }
}
</script>

<style>

</style>
