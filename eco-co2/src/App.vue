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
          <Card :title="article.title"
                :imgSrc="article.image"
                :imgAlt="article.title"
                :content="article.content"
                :url="article.url"
                @favorite="favorite(article)"
                :isFavorite="isFavorite(article)"
          />      
        </b-col>
      </b-row>
    </b-container>

  </div>
</template>

<script>
import axios from 'axios'
import Card from './components/Card.vue'

export default {
  name: 'app',
  components: {
    Card
  },
  data () {
    return {
      sources: null,
      articles: [],
      food: null,
      favorites: [],
      newFavorite: null
    }
  },
  methods: {
    favorite(article) {
      if (this.isFavorite(article))
      {
        this.favorites.splice(this.favorites.indexOf(article.url), 1);       
      } else {
        this.favorites.push(article.url)
      } 
      this.saveFavorites()   
    },
    isFavorite(article) {
      console.log(article)
      return (this.favorites.indexOf(article.url) != -1)
    },
    saveFavorites() {
      const parsed = JSON.stringify(this.favorites);
      localStorage.setItem('favorites', parsed);
    },    
    chooseSource(event) {
      var _self = this
      axios
        .get('https://newsapi.org/v2/top-headlines?sources='+event.target.value+'&apiKey=cd94713f6fe5467d893b99d5d69b75e5')
        .then(
          function (response) {
            _self.articles = []
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
    if (localStorage.getItem('favorites')) {
      try {
        this.favorites = JSON.parse(localStorage.getItem('favorites'));
      } catch(e) {
        localStorage.removeItem('favorites');
      }
    }    
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
