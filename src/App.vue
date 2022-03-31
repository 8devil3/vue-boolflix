<template>
  <div id="app" class="col">
    <header-site @strSearch="getData" />
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
        this.complete = true
        this.loading = false
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
        if (urlChunk == this.urlMovies) {
          this.arrMovies = response.data.results
          this.getCredits(this.urlMovies, this.arrMovies)
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
          if (urlChunk == this.urlMovies) {
            this.arrMoviesCast = response.data.cast
            this.arrMovies[index].cast = this.arrMoviesCast.slice(0, 5)
          } else {
            this.arrSeriesCast = response.data.cast
            this.arrSeries[index].cast = this.arrSeriesCast.slice(0, 5)
          }
        })
      })
    },
    getAllGenres(urlChunk){
      axios.get(this.baseURL + 'genre/' + urlChunk + 'list', { params: {
      api_key: this.APIkey,
      language: this.language
      }
      })
      .then((response) => {
        if (urlChunk == this.urlMovies) {
          this.arrMoviesGenres = response.data.genres
        } else {
          this.arrSeriesGenres = response.data.genres
        }
      })
    }
  },
  created(){
    this.getAllGenres(this.urlSeries)
    this.getAllGenres(this.urlMovies)
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
