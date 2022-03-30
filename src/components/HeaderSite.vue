<template>
<header class="row justify-between align-center wrap">
    <h1>boolflix</h1>
    <div class="row align-center">
        <input @keyup.enter="emitsData(search)" type="text" id="search" v-model.trim="search">
        <button @click="emitsData(search)">cerca</button>
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
    methods: {
        emitsData(strSearch){
            this.$emit('strSearch', strSearch)
            document.querySelector('#search').value = ''
        }
    }
}
</script>

<style lang="scss" scoped>

header {
    background-color: black;
    padding: 1rem;

    h1 {
        color: red;
        text-transform: uppercase;
        margin: 1rem 0;
    }

    > div {
        border-radius: 100px;
        overflow: hidden;
        flex-shrink: 0;

        input {
            height: 2rem;
            border: 0;
            padding: 0 1rem;
            
            &:focus, &:focus-visible {
                outline: 0;
            }
        }

        button {
            background-color: red;
            color: white;
            padding: 0 0.5rem;
            text-transform: uppercase;
            align-self: stretch;
            cursor: pointer;

            &:hover {
                background-color: rgb(255, 95, 95);
            }
        }
    }
}
</style>
