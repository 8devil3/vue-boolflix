<template>
<main>
    <div class="col container">
        <h2>Film ({{ movieData.length }})</h2>
        <p v-if="movieData.length == 0">Nessun risultato</p>
        <div class="wrapper row wrap align-stretch" v-else>
            <movie-card v-for="movie in movieData" :key="movie.id" :mDetails="movie"/>
        </div>

        <h2>Serie TV ({{ seriesData.length }})</h2>
        <p v-if="seriesData.length == 0">Nessun risultato</p>
        <div class="wrapper row wrap align-stretch" v-else>
            <series-card v-for="serie in seriesData" :key="serie.id" :sDetails="serie" />
        </div>
    </div>
</main>
</template>

<script>
import MovieCard from './MovieCard.vue'
import SeriesCard from './SeriesCard.vue'

export default {
    name: 'MainSite.vue',
    components: {
        SeriesCard,
        MovieCard
    },
    props: {
        movieData: Array,
        seriesData: Array
    }
}
</script>

<style lang="scss" scoped>
main {
    padding: 0 1rem;

    .container {
        max-width: 1680px;

        .wrapper {
            min-height: 512px;
            margin: 0 -0.5rem
        }
        
        h2 {
            font-size: 2.5rem;
            text-transform: uppercase;
            color: #999;
            margin: 2rem 0 1rem 0;
        }
    }
}
</style>
