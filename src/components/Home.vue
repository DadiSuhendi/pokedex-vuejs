<script>
    export default {
        data() {
            return {
                pokemonList: []
            };
        },
        methods: {
            async getListPokemon() {
                let response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=30');
                let data = await response.json();
                let results = data.results;
                
                results.forEach(result => {
                    this.getEachPokemon(result)
                })
            },
            async getEachPokemon(result) {
                let response = await fetch('https://pokeapi.co/api/v2/pokemon/' + result.name);
                let data = await response.json();
                this.pokemonList.sort((a, b) => a.id - b.id)
                this.pokemonList.push(data)
            }
        },
        mounted() {
            this.getListPokemon();
        }
    }
</script>

<template>
    <div class="container">
        <div class="header">
            <div class="title-section">
                <img src="../assets/Pokeball.png" alt="">
                <div class="app-name">Pokedex</div>
            </div>
            <div class="search-section">
                <div class="search">
                    <label for="" class="icon-search">
                        <img src="../assets/Vector.png" alt="">
                    </label>
                    <input type="search" class="input-search" placeholder="Search">
                </div>
                <div class="sort">
                    <button>#</button>
                </div>
            </div>
        </div>
        <div class="main">
            <!-- Cards -->
            <div class="big-card">
                <div v-for="(pokemon, index) in pokemonList" style="margin-bottom: 20px;">
                    <a href="">
                        <div class="card">
                            <div class="pokemon-number">#{{index+1}}</div>
                            <img :src="pokemon.sprites.front_default" alt="" class="img-pokemon">
                            <div class="bg-color">
                                <div class="pokemon-name">{{ pokemon.name }}</div>
                            </div>
                        </div>
                    </a>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .container {
        width: 100%;
        height: 100%;
        background-color: var(--identity-primary);
    }
    .header {
        padding-left: 12px;
        padding-right: 12px;
        padding-top: 12px;
        padding-bottom: 24px;
    }
    .title-section {
        display: flex;
        align-items: center;
        gap: 16px;
        padding-bottom: 12px;
    }
    .app-name {
        font-size: 24px;
        font-weight: bold;
        color: white;
    }
    /* Search Section */
    .search {
        width: 100%;
    }
    .search-section {
        display: flex;
        gap: 16px;
    }
    .input-search {
        width: 100%;
        border-radius: 16px;
        height: 32px;
        border: none;
        padding-left: 35px;
        padding-right: 16px;
        padding-top: 8px;
        padding-bottom: 8px;
    }
    .input-search::placeholder {
        color: var(--grayscale-medium);
    }
    .icon-search {
        position: absolute;
        top: 61px;
        left: 25px;
        cursor: pointer;
    }
    .sort button {
        display: block;
        width: 32px;
        height: 32px;
        border-radius: 16px;
        border: none;
        color: var(--identity-primary);
        background-color: var(--grayscale-white);
        cursor: pointer;
        font-weight: bold;
    }
</style>