<template>
<article class="col">
    <div class="img-box col">
        <img v-if="mDetails.poster_path != null" :src="imgBaseURL + imgSizeMedium + mDetails.poster_path" :alt="mDetails.title">
        <p v-else class="no-img">Locandina non disponibile</p>
    </div>
    <div class="data">
        <p><strong>Titolo: </strong>{{ mDetails.title }}</p>
        <p><strong>Titolo originale: </strong>{{ mDetails.original_title }}</p>

        <p v-if="mDetails.original_language == '' || mDetails.original_language == 'xx'">Nessuna lingua</p>
        <p v-else><strong>Lingua: </strong><lang-flag :iso="mDetails.original_language" :squared="false" /></p>

        <p>
            <strong>Rating: </strong>
            <i class="fa-solid fa-star" v-for="fullStar in setRating(mDetails.vote_average)"></i>
            <i class="fa-regular fa-star" v-for="emptyStar in 5 - setRating(mDetails.vote_average)"></i>
        </p>
        
        <p v-if="mDetails.overview != ''" class="overview"><strong>Trama: </strong>{{ mDetails.overview }}</p>
        <p v-else class="overview"><em>Trama non disponibile</em></p>
    </div>
</article>
</template>

<script>
import LangFlag from 'vue-lang-code-flags'

export default {
    name: 'MovieCard.vue',
    components: {
        LangFlag //https://github.com/qWici/vue-lang-code-flags
    },
    props: {
        mDetails: Object
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
