<template>
  <div id="app">
    <b-navbar toggleable="md" type="dark" variant="primary">

      <b-navbar-brand href="#">Eco Co2 News</b-navbar-brand>

        <b-navbar-nav class="ml-auto">
          <b-button v-b-modal.modalFavorite class="float-right" variant="primary">
            Favorites
            <font-awesome-icon icon="star" color="yellow" />
          </b-button>
        </b-navbar-nav>

    </b-navbar>    
    <b-container fluid>
      <b-row class="mt-2">
        <b-col cols="6">
          <form>
            <div class="form-group">
              <select class="form-control" @change="chooseSource($event)">
                <option v-for="item in sources" :value="item.id" :key="item.id">
                  {{ item.name }}
                </option>
              </select>
            </div>            
          </form> 
        </b-col>  
      </b-row> 
      <b-row>
        <b-col cols="12" md="4" lg="3"v-for="article in articles">
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
    <b-modal id="modalFavorite" size="lg" title="Mes favories" cancel-disabled ok-disabled>
      <div class="table-responsive">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Titre</th>
              <th>Action</th>
            </tr>
          </thead>  
          <tbody>
            <tr v-for="article in favorites">
              <td>{{article.title}}</td>
              <td>
                <b-button class="float-right" variant="primary" size="sm"
                  @click="favorite(article)" >
                  <font-awesome-icon v-if="isFavorite(article)" icon="star" color="yellow" />
                  <font-awesome-icon v-else icon="star" color="white" />
                </b-button>
              </td>
            </tr>
          </tbody>  
        </table>
      </div>
      <div slot="modal-footer" class="w-100">
      </div>      
    </b-modal>
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
        this.favorites.splice(this.favorites.indexOf(article), 1);       
      } else {
        this.favorites.push(article)
      } 
      this.saveFavorites()   
    },
    isFavorite(article) {
      var find = false
      this.favorites.forEach(function(element) {
        if (element.url == article.url) { 
          find = true
        }
      })
      return find
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
