<template>
    <div class="search-container">
        <div class="search-controls">
            <b-form-input v-model="searchText" placeholder="search..."></b-form-input>
            <b-button class="ml-1" v-on:click="searchPokemon">Search</b-button>
        </div>
        <div class="search-results" v-if="searchResults.length > 0">
            <div class="result" :key="result.id" v-for="result in searchResults" v-on:click="selectResult(result.name)">
                {{ result.name }} #{{ result.id }}
            </div>
        </div>
        <b-spinner v-if="isLoading"></b-spinner>
        <Card v-else v-bind:pokemon="pokemon" />
    </div>
</template>

<script>
import Card from '@/components/Card.vue'
import PokeAPI from "pokeapi-typescript";
import AllPokemon from "@/data/pokemonIdAndName.json";

export default {
    name: 'SearchView',
    components: {
        Card
    },
    data() {
        return {
            isLoading: false,
            searchText: '',
            searchResults: [],
            allPokemon: AllPokemon,
            pokemon: null
        }
    },
    methods: {
        searchAutoComplete(searchText) {
            this.searchResults = this.allPokemon.filter((pokemon) => {
                return pokemon.name.toLowerCase().includes(searchText.toLowerCase())
            })
            this.searchResults = searchText !== '' ? this.searchResults.slice(0, 10) : []
        },
        selectResult(searchResult) {
            this.searchText = searchResult
            this.searchPokemon()
            this.searchResults = []
        },
        searchPokemon() {
            this.isLoading = true
            PokeAPI.Pokemon.fetch(this.searchText).then(result => {
                this.pokemon = result
                this.isLoading = false
            }).catch(err => {
                console.log(err)
                this.isLoading = false
            })
        }
    },
    watch: {
        searchText: function(newVal) {
            this.searchAutoComplete(newVal)
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

        .result {
            cursor: pointer;
            padding: 4px;
            background-color: whitesmoke;

            &:hover {
                background-color: gainsboro;
            }
        }
    }
}
</style>
