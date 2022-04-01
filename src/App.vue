<template>
  <div id="app" class="col">
    <header-site @strSearch="getData" :loading="loading"/>
    <main-site v-if="complete" :arrMovies="arrMovies" :arrSeries="arrSeries" :keyword="keyword"/>
    <div v-else-if="loading" class="loading row align-center justify-center"><i class="fa-solid fa-spinner fa-pulse"></i></div>
    <div v-else></div>
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
      urlMovies: 'movie/',
      urlSeries: 'tv/',
      language: 'it-IT',
      adulte: false,
      arrMovies: [],
      arrSeries: [],
      arrMoviesCast: [],
      arrSeriesCast: [],
      arrMoviesGenres: [],
      arrSeriesGenres: [],
      keyword: '',
      complete: false,
      loading: false
    }
  },
  methods: {
    getData(str){
      this.complete = false
      this.arrMovies = []
      this.arrSeries = []

      this.keyword = str

      if (str == '' || str == null) {
        //nothing
      } else {
        this.getMoviesSeries(str, this.urlMovies)
        this.getMoviesSeries(str, this.urlSeries)

        this.loading = true

        setTimeout(() => {
          this.loading = false
          this.complete = true
        }, 1500);
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
        switch (urlChunk) {
          case this.urlMovies:
            this.arrMovies = response.data.results
            this.getCredits(this.urlMovies, this.arrMovies)
            this.getGenres(this.urlMovies, this.arrMovies)
            break;
        
          case this.urlSeries:
            this.arrSeries = response.data.results
            this.getCredits(this.urlSeries, this.arrSeries)
            this.getGenres(this.urlSeries, this.arrSeries)
            break;
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
          switch (urlChunk) {
            case this.urlMovies:
              this.arrMoviesCast = response.data.cast
              this.arrMovies[index].cast = this.arrMoviesCast.slice(0, 5)
              break;
          
            case this.urlSeries:
              this.arrSeriesCast = response.data.cast
              this.arrSeries[index].cast = this.arrSeriesCast.slice(0, 5)
              break;
          }
        })
      })
    },
    getGenres(urlChunk, arrData){
      arrData.forEach((item, index) => {
        axios.get(this.baseURL + urlChunk + item.id, { params: {
            api_key: this.APIkey,
            language: this.language,
          }
          })
        .then((response) => {
          switch (urlChunk) {
            case this.urlMovies:
              this.arrMoviesGenres = response.data.genres
              this.arrMovies[index].genres = this.arrMoviesGenres
              break;
          
            case this.urlSeries:
              this.arrSeriesGenres = response.data.genres
              this.arrSeries[index].genres = this.arrSeriesGenres
              break;
          }
        })
      })
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

  .loading {
    font-size: 16rem;
    text-transform: uppercase;
    font-weight: 700;
    color: #666;
    flex-grow: 1;
    }
}
</style>
