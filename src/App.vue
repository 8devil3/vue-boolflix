<template>
  <div id="app">
    <header-site @strSearch="getData" />
    <main-site :arrMovies="arrMovies" :arrSeries="arrSeries" :keyword="keyword"/>
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
      baseURL: 'https://api.themoviedb.org/3/',
      APIkey: '2a1eafb77e5173892c5f55c2d7d7a8c8',
      urlMovie: 'movie/',
      urlSeries: 'tv/',
      language: 'it-IT',
      adulte: false,
      arrMovies: [],
      arrSeries: [],
      arrMoviesCast: [],
      arrSeriesCast: [],
      keyword: ''
    }
  },
  methods: {
    getData(str){
      this.keyword = str

      if (str == '' || str == null) {
        //nothing
      } else {
        this.getMoviesSeries(str, this.urlMovie)
        this.getMoviesSeries(str, this.urlSeries)
      }
    },
    getMoviesSeries(str, urlChunk){
      axios.get(this.baseURL + 'search/' + urlChunk, { params: {
        api_key: this.APIkey,
        language: this.language,
        query: str,
        include_adult: this.adulte
      }
      })
      .then((response) => {
        if (urlChunk == this.urlMovie) {
          this.arrMovies = response.data.results
          this.getCredits(this.urlMovie, this.arrMovies)
        } else {
          this.arrSeries = response.data.results
          this.getCredits(this.urlSeries, this.arrSeries)
        }
      })
    },
    getCredits(urlChunk, arrData){
      arrData.forEach((item, index) => {
        axios.get(this.baseURL + urlChunk + item.id + '/credits', { params: {
            api_key: this.APIkey,
            language: this.language,
          }
          })
        .then((response) => {
          if (urlChunk == this.urlMovie) {
            this.arrMoviesCast = response.data.cast
            this.arrMovies[index].cast = this.arrMoviesCast.slice(0, 5)
          } else {
            this.arrSeriesCast = response.data.cast
            this.arrSeries[index].cast = this.arrSeriesCast.slice(0, 5)
          }
        })
      })
    },
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
