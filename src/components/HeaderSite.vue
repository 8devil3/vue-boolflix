<template>
<header class="row justify-between align-center">
    <h1>boolflix</h1>
    <div class="row align-stretch align-center">
        <input @keyup.enter="$emit('strSearch', strSearch)" type="text" id="search" v-model="strSearch">
        <button @click="$emit('strSearch', strSearch)">cerca</button>
    </div>
</header>
</template>

<script>
export default {
    name: 'HeaderSite.vue',
    data(){
        return {
            strSearch: ''
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
    }

    input {
        height: 1.5rem;
        border: 0;
        padding: 0 0.5rem;
    }

    button {
        background-color: red;
        color: white;
        padding: 0 0.5rem;
        text-transform: uppercase;
        cursor: pointer;
    }
}
</style>
