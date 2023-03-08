<template>
    <div id="pokemon">
        <div class="card">
            <div class="card-image">
                <figure>
                    <img :src="currentImg" alt="Pokemon" />
                </figure>
            </div>
            <div class="card-content">
                <div class="media">
                    <div class="media-left"></div>
                    <div class="media-content">
                        <p class="title is-4">{{ num }} - {{ upper(name) }}</p>
                        <p class="subtitle is-5">{{ pokemon.type }}</p>
                    </div>
                </div>
                <div class="content">
                    <button class="button is-fullwidth is-rounded" @click="changeSprite">
                        Girar
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    created: function () {
        axios.get(this.url).then((res) => {
            this.pokemon.type = res.data.types[0].type.name
            this.pokemon.front = res.data.sprites.front_default
            this.pokemon.back = res.data.sprites.back_default
            this.currentImg = this.pokemon.front
        })
    },
    data() {
        return {
            isFront: true,
            currentImg: '',
            pokemon: {},
        }
    },
    props: {
        num: Number,
        name: String,
        url: String,
    },
    methods: {
        upper: function (value) {
            var newName = value[0].toUpperCase() + value.slice(1)
            return newName
        },
        changeSprite: function () {
            if (this.isFront) {
                this.isFront = false
                this.currentImg = this.pokemon.back
            } else {
                this.isFront = true
                this.currentImg = this.pokemon.front
            }
        },
    },
}
</script>

<style>
#pokemon {
    margin-top: 3%;
}
</style>