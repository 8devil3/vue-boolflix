<template>
<header class="row justify-between align-center wrap">
    <h1>boolflix</h1>
    <div class="row align-center">
        <input @keyup.enter="emitsData(search)" type="text" id="search" v-model.trim="search" placeholder="Cerca un film o una serie" :disabled="loading">
        <i class="fa-solid fa-xmark" @click="resetInput"></i>
        <button @click="emitsData(search)" :disabled="loading"><i class="fa-solid fa-magnifying-glass"></i></button>
    </div>
</header>
</template>

<script>
export default {
    name: 'HeaderSite.vue',
    data(){
        return {
            search: ''
        }
    },
    props: {
        loading: Boolean
    },
    methods: {
        emitsData(strSearch){
            this.$emit('strSearch', strSearch)
        },
        resetInput(){
            document.querySelector('#search').value = ''
        }
    }
}
</script>

<style lang="scss" scoped>

header {
    background-color: black;
    height: 6rem;
    padding: 0 1rem;
    position: fixed;
    width: 100%;
    top: 0;
    z-index: 10;

    h1 {
        color: red;
        text-transform: uppercase;
        margin: 1rem 0;
    }

    > div {
        border-radius: 100px;
        overflow: hidden;
        flex-shrink: 0;
        position: relative;

        input {
            height: 2rem;
            min-width: 14rem;
            border: 0;
            padding: 0 1rem;
            
            &:focus, &:focus-visible {
                outline: 0;
            }

            &:disabled {
                color: #999;
                background-color: #666;
            }
        }

        .fa-xmark {
            position: absolute;
            right: 3rem;
            top: 50%;
            transform: translate(0, -50%);
            color: #999;
            padding: 0.25rem;
            cursor: pointer;
        }

        button {
            background-color: red;
            color: white;
            padding: 0 0.8rem;
            text-transform: uppercase;
            align-self: stretch;
            cursor: pointer;

            &:hover {
                background-color: rgb(255, 95, 95);
            }

            &:hover:disabled, &:disabled {
                background-color: #921414;
                color: #666
            }
        }
    }
}
</style>
