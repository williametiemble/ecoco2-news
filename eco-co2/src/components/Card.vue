<template>
  <div class="card">
    <b-card :title="title"
            :img-src="imgSrc"
            :img-alt="imgAlt"
            img-top
            class="mb-2">
      <p class="card-text">
        {{content}}
      </p>
      <b-button :href="url" variant="primary" size="sm">Voir la news</b-button>
      <b-button class="float-right" variant="primary" size="sm"
        @click="favorite" >
        <font-awesome-icon v-if="isFavorite" icon="star" color="yellow" />
        <font-awesome-icon v-else icon="star" color="white" />
      </b-button>
    </b-card>  
  </div>
</template>

<script>
export default {
  name: 'Card',
  methods: {
    favorite() {
      this.$emit('favorite');
    }
  },  
  props: {
    title: String,
    imgSrc: String,
    imgAlt: String,
    content: String,
    url: String,
    isFavorite: Boolean
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
