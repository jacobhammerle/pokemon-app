<template>
    <div class="search-container">
        <div class="search-controls">
            <b-form-input v-model="searchText" placeholder="search..."></b-form-input>
            <b-button class="ml-1" v-on:click="searchPokemon(searchText)">Search</b-button>
        </div>
        <div class="search-results">
            <b-spinner v-if="isLoading"></b-spinner>
            <Card v-else v-bind:pokemon="pokemon" />
        </div>
    </div>
</template>

<script>
import Card from '@/components/Card.vue'
import PokeAPI from "pokeapi-typescript";

export default {
    name: 'SearchView',
    components: {
        Card
    },
    data() {
        return {
            isLoading: false,
            searchText: '',
            pokemon: null
        }
    },
    methods: {
        searchPokemon(searchText) {
            this.isLoading = true
            PokeAPI.Pokemon.fetch(searchText).then(result => {
                this.pokemon = result
                this.isLoading = false
            }).catch(err => {
                console.log(err)
                this.isLoading = false
            })
        }
    }
}
</script>

<style lang="less">
.search-container {
    .search-controls {
        display: flex;
        justify-content: space-between;
        align-items: center;
        max-width: 400px;
        margin: 16px auto;
    }
    .search-results {
        max-width: 600px;
        margin: 0 auto;
    }
}
</style>
