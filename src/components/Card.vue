<template>
    <div v-if="pokemon">
        <b-card
            no-body
            :img-src="pokemon.sprites.front_default"
            :img-alt="pokemon.name"
            img-top
        >
            <b-card-body>
                <b-card-title>{{ pokemon.name }}</b-card-title>
                <b-card-sub-title class="mb-2">#{{ pokemon.id }}</b-card-sub-title>
            </b-card-body>

            <b-list-group flush>
                <b-list-group-item :key="type.type.name" v-for="type in pokemon.types">
                    {{ type.type.name }}
                </b-list-group-item>
            </b-list-group>
        </b-card>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import {IPokemon} from "pokeapi-typescript";

@Component export default class Card extends Vue {
    @Prop() private pokemon!: IPokemon;
}
</script>

<style lang="less">
.card {
    max-width: 600px;
    margin: 0 auto;

    img {
        width: 100px;
        margin: 0 auto;
    }
}
</style>
