<template>
  <div id="app">
    <header-site @strSearch="getData" />
    <main-site :arrMovies="arrMovies" :arrSeries="arrSeries"/>
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
      baseURL: 'https://api.themoviedb.org/3/search/multi/',
      APIkey: '2a1eafb77e5173892c5f55c2d7d7a8c8',
      language: 'it-IT',
      adulte: false,
      arrData: [],
      arrMovies: [],
      arrSeries: []
    }
  },
  methods: {
    getData(str){
      if (str == '' || str == null) {
        this.resetSearch() //riporto lo stato iniziale in cui i film sono assenti
      } else {
        axios.get(this.baseURL, { params: {
          api_key: this.APIkey,
          language: this.language,
          query: str,
          include_adult: this.adulte
        }
        })
        .then((response) => {
          this.arrData = response.data.results
          this.setMovieSeries()
        })
      }

    },
    setMovieSeries(){
      this.arrMovies = []
      this.arrSeries = []

      this.arrData.forEach(item => {
          if (item.media_type == "movie") {
              this.arrMovies.push(item)
          } else if (item.media_type == "tv") {
            this.arrSeries.push(item)
          } else {
            //nothing
          }
      })
    },
    resetSearch(){
      this.arrMovies = []
      this.arrSeries = []
    }
  }
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
