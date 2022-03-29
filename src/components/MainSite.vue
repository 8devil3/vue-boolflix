<template>
<main>
    <h2>film</h2>
    <ul>
        <li v-for="mDetails in movieData" :key="mDetails.id">
            <p>{{ mDetails.title }}</p>
            <p>{{ mDetails.original_title }}</p>
            <p>{{ mDetails.original_language }}</p>
            <p v-if="mDetails.original_language == '' || mDetails.original_language == 'xx'">Nessuna lingua</p>
            <lang-flag v-else :iso="mDetails.original_language" :squared="false" />
            <p>{{ mDetails.vote_average }}</p>
            <img :src="imgBaseURL + imgSizeSmall + mDetails.poster_path" :alt="mDetails.title">
        </li>
    </ul>
    <h2>serie TV</h2>
        <ul>
        <li v-for="sDetails in seriesData" :key="sDetails.id">
            <p>{{ sDetails.name }}</p>
            <p>{{ sDetails.original_name }}</p>
            <p>{{ sDetails.original_language }}</p>
            <p v-if="sDetails.original_language == '' || sDetails.original_language == 'xx'">Nessuna lingua</p>
            <lang-flag v-else :iso="sDetails.original_language" :squared="false" />
            <p>{{ sDetails.vote_average }}</p>
            <img :src="imgBaseURL + imgSizeSmall + sDetails.poster_path" :alt="sDetails.title">
        </li>
    </ul>
</main>
</template>

<script>
import LangFlag from 'vue-lang-code-flags'

export default {
    name: 'MainSite.vue',
    components: {
        LangFlag //https://github.com/qWici/vue-lang-code-flags
    },
    data(){
        return {
            imgBaseURL: 'https://image.tmdb.org/t/p/',
            imgSizeSmall: 'w154',
            imgSizeMedium: 'w342',
            imgSizeLarge: 'w500',
            imgSizeBig: 'w780',
            imgURL: '',
        }
    },
    props: {
        movieData: Array,
        seriesData: Array
    }
}
</script>

<style>

</style>
