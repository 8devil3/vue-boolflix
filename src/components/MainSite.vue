<template>
    <ul>
        <li v-for="details in data" :key="details.id">
            <p>{{ details.title }}</p>
            <p>{{ details.original_title }}</p>
            <p>{{ details.original_language }}</p>
            <p>{{ details.vote_average }}</p>
        </li>
    </ul>
</template>

<script>
export default {
    name: 'MainSite.vue',
    data(){
        return {

        }
    },
    props: {
        data: Array
    }
}
</script>

<style>

</style>
