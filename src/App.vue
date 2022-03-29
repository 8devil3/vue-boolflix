<template>
  <div id="app">
    <header-site @strSearch="getData" />
    <main-site :movieData="movieData" :seriesData="seriesData" />
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
      movieData: [],
      seriesData: []
    }
  },
  methods: {
    getData(str){
      this.search = str

      axios.get(this.baseURL + this.movieURL + this.APIkey + '&language=it-IT&query=' + this.search + '&include_adult=false')
      .then((response) => { this.movieData = response.data.results })

      axios.get(this.baseURL + this.seriesURL + this.APIkey + '&language=it-IT&query=' + this.search + '&include_adult=false')
      .then((response) => { this.seriesData = response.data.results })
    }
  },
}
</script>

<style lang="scss">
@import './assets/global.scss';

#app {
  font-family: Helvetica, Arial, sans-serif;
  color: #e6e6e6;
  background-color: #333;
  min-height: 100vh;
}
</style>
