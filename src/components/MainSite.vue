<template>
<main>
    <div class="col container">
        <p v-if="keyword != ''" class="keyword">hai cercato: <em>{{ keyword }}</em></p>
        <p v-else></p>

        <h2>Film ({{ arrMovies.length }})</h2>
        <p v-if="arrMovies.length == 0">Nessun risultato</p>
        <div class="wrapper row wrap align-stretch" v-else>
            <card-data v-for="movieShow in arrMovies" :key="movieShow.id" :details="movieShow"/>
        </div>

        <hr>

        <h2>Serie TV ({{ arrSeries.length }})</h2>
        <p v-if="arrSeries.length == 0">Nessun risultato</p>
        <div class="wrapper row wrap align-stretch" v-else>
            <card-data v-for="tvShow in arrSeries" :key="tvShow.id" :details="tvShow"/>
        </div>
    </div>
</main>
</template>

<script>
import CardData from './CardData.vue'

export default {
    name: 'MainSite.vue',
    components: {
        CardData,
    },
    props: {
        arrMovies: Array,
        arrSeries: Array,
        keyword: String
    }
}
</script>

<style lang="scss" scoped>
main {
    padding: 0 1rem;

    .container {

        .keyword {
            margin-top: 1rem;
            font-size: 1.2rem;
            color: #999;
            text-transform: uppercase;

            em {
                color: red;
            }
        }

        .wrapper {
            min-height: 512px;
            margin: 0 -0.5rem
        }

        hr {
            border: 0;
            background-color: #666;
            height: 2px;
            margin: 3rem 0 1.5rem 0;
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
