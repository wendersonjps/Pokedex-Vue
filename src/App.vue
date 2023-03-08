<template>
    <div id="app">
        <div class="column is-half is-offset-one-quarter">
            <img src="./assets/pokedex.png" alt="Logo" />
            <input
                class="input is-normal is-warning"
                type="text"
                placeholder="Buscar pokémon"
                v-model="search"
            />
            <div v-for="(poke, index) in searchResult" :key="poke.url">
                <PokemonList
                    :num="index + 1"
                    :name="poke.name"
                    :url="poke.url"
                />
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

import PokemonList from './components/PokemonList.vue'

export default {
    name: 'App',
    data() {
        return {
            pokemons: [],
            search: '',
        }
    },
    created: function () {
        axios
            .get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0')
            .then((res) => {
                this.pokemons = res.data.results
            })
            .catch((err) => {
                console.log(err)
            })
    },
    components: {
        PokemonList,
    },
    computed: {
        searchResult: function () {
            if (this.search == '' || this.search == ' ') {
                return this.pokemons
            } else {
                return this.pokemons.filter((pokemon) =>
                    pokemon.name
                        .toLowerCase()
                        .includes(this.search.toLowerCase())
                )
            }
        },
    },
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}

#searchBtn {
    margin-top: 1%;
}
</style>
