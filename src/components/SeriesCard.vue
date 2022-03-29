<template>
<article class="col">
    <div class="img-box col">
        <img v-if="sDetails.poster_path != null" :src="imgBaseURL + imgSizeMedium + sDetails.poster_path" :alt="sDetails.title">
        <p v-else class="no-img">Locandina non disponibile</p>
    </div>
    <div class="data">
        <p><strong>Titolo: </strong>{{ sDetails.name }}</p>
        <p><strong>Titolo originale: </strong>{{ sDetails.original_name }}</p>

        <p v-if="sDetails.original_language == '' || sDetails.original_language == 'xx'">Nessuna lingua</p>
        <p v-else><strong>Lingua: </strong><lang-flag :iso="sDetails.original_language" :squared="false" /></p>

        <p>
            <strong>Rating: </strong>
            <i class="fa-solid fa-star" v-for="fullStar in setRating(sDetails.vote_average)"></i>
            <i class="fa-regular fa-star" v-for="emptyStar in 5 - setRating(sDetails.vote_average)"></i>
        </p>
        
        <p class="overview"><strong>Trama: </strong>{{ sDetails.overview }}</p>
    </div>
</article>
</template>

<script>
import LangFlag from 'vue-lang-code-flags'

export default {
    name: 'SeriesCard.vue',
    components: {
        LangFlag //https://github.com/qWici/vue-lang-code-flags
    },
    props: {
        sDetails: Object
    },
    data(){
        return {
            imgBaseURL: 'https://image.tmdb.org/t/p/',
            imgSizeSmall: 'w154',
            imgSizeMedium: 'w342',
            imgSizeLarge: 'w500',
            imgSizeBig: 'w780',
            imgURL: ''
        }
    },
    methods: {
        setRating(rating){
            return Math.ceil(rating / 2)
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../assets/_card.scss';
</style>
