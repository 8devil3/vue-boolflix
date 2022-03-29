<template>
<header>
    <input @keyup.enter="$emit('strSearch', strSearch)" type="text" id="search" v-model="strSearch">
    <button @click="$emit('strSearch', strSearch)">cerca</button>
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

<style>

</style>
