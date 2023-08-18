<script>
    import Detail from './Detail.vue'
    import { ref } from 'vue'
    export default {
        data() {
            return {
                pokemonList: [],
                viewAsDetail: false,
                selectedPokemon: '',
                headerDetail: {
                    background: '--pokemon-type-'
                },
                pokemonTypes: [],
                search: ''
            };
        },
        methods: {
            async getListPokemon() {
                let response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=151');
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
            },
            async viewDetailPokemon(pokemonName) {
                this.viewAsDetail = true
                let response = await fetch('https://pokeapi.co/api/v2/pokemon/' + pokemonName);
                let data = await response.json();
                this.selectedPokemon = data
            },
            getTypeColor(type) {
                return `var(--pokemon-type-${type})`;
            },
            async nextPokemon(id) {
                this.viewAsDetail = true
                let pokemonId = id+1;
                let response = await fetch('https://pokeapi.co/api/v2/pokemon/' + pokemonId);
                let data = await response.json();
                this.selectedPokemon = data
            },
            async previousPokemon(id) {
                this.viewAsDetail = true
                let pokemonId = id-1;
                let response = await fetch('https://pokeapi.co/api/v2/pokemon/' + pokemonId);
                let data = await response.json();
                this.selectedPokemon = data
            },
            showHome() {
                this.viewAsDetail = false
                this.search = ''
            }
        },
        mounted() {
            this.getListPokemon();
        },
        watch: {
            search() {
                let filteredPokemon = this.pokemonList.filter(item => {
                    return item.name.includes(this.search.toLowerCase())
                })
                if(this.search === '') {
                    this.getListPokemon();
                } else {
                    this.pokemonList = filteredPokemon;
                }
            }
        },
        computed: {
            headerStyle() {
                if (this.selectedPokemon && this.selectedPokemon.types) {
                    const type = this.selectedPokemon.types[0].type.name;
                    const background = `var(${this.headerDetail.background}${type})`;
                    return {
                        backgroundColor: background
                    };
                }
                return {};
            },
            colorStyle() {
                if (this.selectedPokemon && this.selectedPokemon.types) {
                    const type = this.selectedPokemon.types[0].type.name;
                    const color = `var(${this.headerDetail.background}${type})`;
                    return {
                        color: color
                    };
                }
                return {};
            },
            hpBar() {
                if (this.selectedPokemon && this.selectedPokemon.stats) {
                    let baseStat = this.selectedPokemon.stats[0].base_stat;
                    if(baseStat > 100) {
                        baseStat = 100
                    }
                    return {
                        width: `${baseStat}%`
                    };
                }
                return {};
            },
            hpStyles() {
               return { ...this.hpBar, ...this.headerStyle };
            },
            atkBar() {
                if (this.selectedPokemon && this.selectedPokemon.stats) {
                    let baseStat = this.selectedPokemon.stats[1].base_stat;
                    if(baseStat > 100) {
                        baseStat = 100
                    }
                    return {
                        width: `${baseStat}%`
                    };
                }
                return {};
            },
            atkStyles() {
               return { ...this.atkBar, ...this.headerStyle };
            },
            defBar() {
                if (this.selectedPokemon && this.selectedPokemon.stats) {
                    let baseStat = this.selectedPokemon.stats[2].base_stat;
                    if(baseStat > 100) {
                        baseStat = 100
                    }
                    return {
                        width: `${baseStat}%`
                    };
                }
                return {};
            },
            defStyles() {
               return { ...this.defBar, ...this.headerStyle };
            },
            satkBar() {
                if (this.selectedPokemon && this.selectedPokemon.stats) {
                    let baseStat = this.selectedPokemon.stats[3].base_stat;
                    if(baseStat > 100) {
                        baseStat = 100
                    }
                    return {
                        width: `${baseStat}%`
                    };
                }
                return {};
            },
            satkStyles() {
               return { ...this.satkBar, ...this.headerStyle };
            },
            sdefBar() {
                if (this.selectedPokemon && this.selectedPokemon.stats) {
                    let baseStat = this.selectedPokemon.stats[4].base_stat;
                    if(baseStat > 100) {
                        baseStat = 100
                    }
                    return {
                        width: `${baseStat}%`
                    };
                }
                return {};
            },
            sdefStyles() {
               return { ...this.sdefBar, ...this.headerStyle };
            },
            spdBar() {
                if (this.selectedPokemon && this.selectedPokemon.stats) {
                    let baseStat = this.selectedPokemon.stats[5].base_stat;
                    if(baseStat > 100) {
                        baseStat = 100
                    }
                    return {
                        width: `${baseStat}%`
                    };
                }
                return {};
            },
            spdStyles() {
               return { ...this.spdBar, ...this.headerStyle };
            },
        },
        components: {
            Detail
        }
    }
</script>

<template>
    <div class="container" v-if="!viewAsDetail">
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
                    <input type="search" v-model="search" class="input-search" placeholder="Search">
                </div>
                <!-- <div class="sort">
                    <button>#</button>
                </div> -->
            </div>
        </div>
        <div class="main">
            <!-- Cards -->
            <div class="big-card">
                <div v-for="(pokemon, index) in pokemonList" style="margin-bottom: 20px;" class="container-card" @click="viewDetailPokemon(pokemon.name)">
                    <div class="card-content">
                        <div class="card">
                            <div class="pokemon-number">#{{index+1}}</div>
                            <img :src="pokemon.sprites.front_default" alt="" class="img-pokemon">
                            <div class="bg-color">
                                <div class="pokemon-name">{{ pokemon.name }}</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- DETAIL POKEMON -->
    <div class="view-detail" v-else>
        <Detail 
            :headerStyle="headerStyle" 
            :showHome="showHome" 
            :previousPokemon="previousPokemon" 
            :nextPokemon="nextPokemon" 
            :getTypeColor="getTypeColor"
            :colorStyle="colorStyle"
            :hpStyles="hpStyles"
            :atkStyles="atkStyles"
            :defStyles="defStyles"
            :satkStyles="satkStyles"
            :sdefStyles="sdefStyles"
            :spdStyles="spdStyles"
            :selectedPokemon="selectedPokemon"
        />
    </div>
</template>

<style scoped>
    .container {
        box-sizing: border-box;
        width: 100%;
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
        position: relative;
    }
    .search-section {
        display: flex;
        gap: 16px;
    }
    .input-search {
        width: 100%;
        height: 32px;
        border-radius: 16px;
        height: 32px;
        border: none;
        padding-left: 35px;
        padding-right: 16px;
        padding-top: 8px;
        padding-bottom: 8px;
        position: relative;
    }
    .input-search::placeholder {
        color: var(--grayscale-medium);
    }
    .icon-search {
        position: absolute;
        z-index: 99;
        top: 8px;
        left: 14px;
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