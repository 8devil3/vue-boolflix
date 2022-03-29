<template>
  <div id="app">
    <header-site @strSearch="getMovieData" />
    <main-site :data="data" />
  </div>
</template>

<script>
import axios from 'axios'
import HeaderSite from './components/HeaderSite.vue'
import MainSite from './components/MainSite.vue'

export default {
  name: 'App',
  components: {
    HeaderSite,
    MainSite
  },
  data(){
    return {
      baseURL: 'https://api.themoviedb.org/3/search/',
      movieURL: 'movie/',
      seriesURL: 'tv/',
      APIkey: '?api_key=2a1eafb77e5173892c5f55c2d7d7a8c8',
      search: '',
      data: []
    }
  },
  methods: {
    getMovieData(str){
      this.search = str
      axios.get(this.baseURL + this.movieURL + this.APIkey + '&query=' + this.search)
      .then((response) => { this.data = response.data.results })
    }
  },
}
</script>

<style lang="scss">
#app {
  font-family: Helvetica, Arial, sans-serif;
}
</style>
