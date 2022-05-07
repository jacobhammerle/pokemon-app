<template>
    <div class="home-container" v-if="allPokemon && allPokemon.length > 0">
        <b-table striped hover :fields="fields" :items="allPokemon"></b-table>
    </div>
    <div v-else>
        no pokemon yet...
    </div>
</template>

<script>
import PokeAPI from "pokeapi-typescript";

export default {
    name: 'HomeView',
    components: {},
    data() {
        return {
            isLoading: false,
            fields: [
                {
                    key: 'id',
                    sortable: true
                },
                {
                    key: 'name',
                    sortable: true
                }
            ],
            allPokemon: []
        }
    },
    methods: {
        async getPokemonList() {
            this.isLoading = true

            await PokeAPI.Pokemon.list(151, 0).then(result => {
                for (let pokemon of result.results) {
                    this.getPokemonDetails(pokemon.name).then(result => {
                        this.allPokemon.push({id: result.id, name: result.name})
                    })
                }
                this.isLoading = false
            }).catch(err => {
                console.log(err)
                this.isLoading = false
            })
        },
        async getPokemonDetails(name) {
            return await PokeAPI.Pokemon.resolve(name)
        }
    },
    mounted() {
        this.getPokemonList()
    }
}
</script>

<style lang="less">
.home-container {
    // home view styles go here
}
</style>
