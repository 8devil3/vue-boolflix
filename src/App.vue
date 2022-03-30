<template>
  <div id="app">
    <header-site @strSearch="getData" />
    <main-site :arrMovies="setMovie()" :arrSeries="setSeries()"/>
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
      APIkey: '2a1eafb77e5173892c5f55c2d7d7a8c8',
      language: 'it-IT',
      adulte: false,
      arrData: [],
      arrMovies: [],
      arrSeries: [],
      arrCast: []
    }
  },
  methods: {
    getData(str){
      if (str == '' || str == null) {
        //nothing
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
        })
        .then(() => {
          this.setCredits()
        })
      }
    },
    setMovie(){
      return this.arrData.filter(item => {
        return item.media_type == 'movie'
      })
    },
    setSeries(){
      return this.arrData.filter(item => {
        return item.media_type == 'tv'
      })
    },
    setCredits(){
      this.arrData.forEach((item, index) => {
        this.getCredits(item.id, index)
      })
    },
    getCredits(id, index){
      this.arrCast = []

      axios.get('https://api.themoviedb.org/3/movie/' + id + '/credits?api_key=' + this.APIkey + '&language=it-IT')
      .then((response) => {
        this.arrCast = response.data.cast
        this.arrData[index].cast = this.arrCast.slice(0, 5)
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
}
</style>
