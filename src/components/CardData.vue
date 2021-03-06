<template>
<article>
    <div class="card-wrapper">
        <div class="img-box col">
            <img v-if="details.poster_path != null" :src="imgBaseURL + imgSize + details.poster_path" :alt="details.title">
            <p v-else class="no-img">Locandina non disponibile</p>
        </div>
        <div class="data">
            <p><strong>Titolo: </strong>{{ details.title || details.name }}</p>
            <p><strong>Titolo originale: </strong>{{ details.original_title || details.original_name }}</p>

            <p v-if="details.cast.length > 0"><strong>Cast: </strong><span class="item-list" v-for="actor in details.cast" :key="actor.id">{{ actor.name }}</span></p>
            <p v-else><strong>Cast: </strong><em>non disponibile</em></p>

            <p v-if="details.genres.length > 0"><strong>Genere: </strong><span class="item-list" v-for="genre in details.genres" :key="genre.id">{{ genre.name }}</span></p>
            <p v-else><strong>Genere: </strong><em>non disponibile</em></p>

            <p><strong>Data di uscita: </strong>{{ formatDate(details.release_date) || formatDate(details.first_air_date) }}</p>

            <p v-if="details.original_language == '' || details.original_language == 'xx'"><strong>Lingua: </strong><em>non disponibile</em></p>
            <p v-else><strong>Lingua: </strong><lang-flag :iso="details.original_language" :squared="false" /></p>

            <p>
                <strong>Rating: </strong>
                <i class="fa-solid fa-star" v-for="fullStar in setRating(details.vote_average)"></i>
                <i class="fa-regular fa-star" v-for="emptyStar in 5 - setRating(details.vote_average)"></i>
            </p>
            
            <p v-if="details.overview != ''" class="overview"><strong>Trama: </strong>{{ details.overview }}</p>
            <p v-else><strong>Trama: </strong><em>non disponibile</em></p>
        </div>
    </div>
</article>
</template>

<script>
import LangFlag from 'vue-lang-code-flags'

export default {
    name: 'CardData.vue',
    components: {
        LangFlag //https://github.com/qWici/vue-lang-code-flags
    },
    props: {
        details: Object
    },
    data(){
        return {
            imgBaseURL: 'https://image.tmdb.org/t/p/',
            imgSize: 'w342',
            imgURL: ''
        }
    },
    methods: {
        setRating(rating){
            return Math.ceil(rating / 2)
        },
        formatDate(date){
            if (date != null) {
                return date.slice(-2) + "/" + date.slice(5, 7) + "/" + date.slice(0, 4);
            }
        }
    }
}
</script>

<style lang="scss" scoped>
article {
    width: var(--poster-w);
    height:  var(--poster-h);;
    margin: 0.5rem;
    perspective: 80rem;

    .card-wrapper {
        height: 100%;
        width: 100%;
        position: relative;
        background-color: #1a1a1a;
        border: 1px #666 solid;
        transform-style: preserve-3d;
        transition: transform 0.6s;
    }

    .img-box {
        position: absolute;
        height: 100%;
        width: 100%;
        backface-visibility: hidden;
        transform: rotateX(0deg); //fix per FireFox: gestisce male la proprietà "backface-visibility: hidden"
    }

    img {
        width: 100%;
        height: 100%;
        display: block;
        object-fit: cover;
    }

    .no-img {
        margin: auto;
        text-transform: uppercase;
        font-weight: 700;
        color: #666;
        font-size: 2rem;
        text-align: center;
    }

    .data {
        color: #e6e6e6;
        padding: 1rem;
        overflow-y: auto;
        position: absolute;
        height: 100%;
        width: 100%;
        backface-visibility: hidden;
        transform: rotateY(180deg);

        .overview {
            line-height: 150%;
            align-self: stretch;
        }

        p {
            line-height: 150%;
        }

        p:not(:last-child) {
            margin-bottom: 0.8rem;
        }

        i {
            color: gold;
        }

        .item-list:not(:last-child)::after {
            content: ', ';
        }
    }

    &:hover .card-wrapper {
        transform: rotateY(180deg);
    }

    &:hover .data{
        display: flex;
        flex-direction: column;
    }
}
</style>
