<script>
    import _debounce from 'lodash/debounce';
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
                const type = this.selectedPokemon.types[0].type.name;
                const background = `var(${this.headerDetail.background}${type})`;
                return {
                    backgroundColor: background
                };
            },
            colorStyle() {
                const type = this.selectedPokemon.types[0].type.name;
                const color = `var(${this.headerDetail.background}${type})`;
                return {
                    color: color
                };
            },
            hpBar() {
                let baseStat = this.selectedPokemon.stats[0].base_stat;
                if(baseStat > 100) {
                    baseStat = 100
                }
                return {
                    width: `${baseStat}%`
                };
            },
            hpStyles() {
               return { ...this.hpBar, ...this.headerStyle };
            },
            atkBar() {
                let baseStat = this.selectedPokemon.stats[1].base_stat;
                if(baseStat > 100) {
                    baseStat = 100
                }
                return {
                    width: `${baseStat}%`
                };
            },
            atkStyles() {
               return { ...this.atkBar, ...this.headerStyle };
            },
            defBar() {
                let baseStat = this.selectedPokemon.stats[1].base_stat;
                if(baseStat > 100) {
                    baseStat = 100
                }
                return {
                    width: `${baseStat}%`
                };
            },
            defStyles() {
               return { ...this.defBar, ...this.headerStyle };
            },
            satkBar() {
                let baseStat = this.selectedPokemon.stats[1].base_stat;
                if(baseStat > 100) {
                    baseStat = 100
                }
                return {
                    width: `${baseStat}%`
                };
            },
            satkStyles() {
               return { ...this.satkBar, ...this.headerStyle };
            },
            sdefBar() {
                let baseStat = this.selectedPokemon.stats[1].base_stat;
                if(baseStat > 100) {
                    baseStat = 100
                }
                return {
                    width: `${baseStat}%`
                };
            },
            sdefStyles() {
               return { ...this.sdefBar, ...this.headerStyle };
            },
            spdBar() {
                let baseStat = this.selectedPokemon.stats[1].base_stat;
                if(baseStat > 100) {
                    baseStat = 100
                }
                return {
                    width: `${baseStat}%`
                };
            },
            spdStyles() {
               return { ...this.spdBar, ...this.headerStyle };
            },
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
        <div class="header-detail" :style="headerStyle">
            <div class="back-button" @click="showHome">
                <div>
                    <img src="../assets/arrow-left.png" alt="">
                </div>
                <div class="detail-name">{{ selectedPokemon.name }}</div>
            </div>
            <span class="detail-number">#{{selectedPokemon.id}}</span>
        </div>
        <div class="pokemon-slider" :style="headerStyle">
            <div class="slider-left" v-if="selectedPokemon.id > 1" style="cursor: pointer;" @click="previousPokemon(selectedPokemon.id)">
                <img src="../assets/slider-left.png" alt="">
            </div>
            <div class="slider-left" v-else></div>
            <div class="pokemon">
                <img :src="selectedPokemon.sprites.other['official-artwork'].front_default" alt="" width="200">
            </div>
            <div class="slider-right" style="cursor: pointer;" @click="nextPokemon(selectedPokemon.id)">
                <img src="../assets/slider-right.png" alt="">
            </div>
        </div>
        <div class="pokeball">
            <img src="../assets/Pokeball-bg.png" alt="">
        </div>
        <div class="main-detail" :style="headerStyle">
            <div class="big-card-detail">
                <div class="badge-section">
                    <div 
                        class="badge" 
                        v-for="(item, index) in selectedPokemon.types"
                        :style="{ backgroundColor: getTypeColor(item.type.name) }"
                    >{{ item.type.name }}</div>
                </div>
                <div class="about-section" :style="colorStyle">
                    <p>About</p>
                </div>
                <div class="attribute-section">
                    <table width="100%" cellpadding="0" cellspacing="5">
                        <tr>
                            <td width="70">
                                <div class="attribute-main">
                                    <div>
                                    <img src="../assets/weight.png" alt="">
                                    </div>
                                    <p>{{selectedPokemon.weight / 10}} kg</p>
                                </div>
                            </td>
                            <td rowspan="2" class="rowspan" width="10"><hr></td>
                            <td width="70">
                                <div class="attribute-main">
                                    <div>
                                    <img src="../assets/height.png" alt="">
                                    </div>
                                    <p>{{ selectedPokemon.height / 10 }} m</p>
                                </div>
                            </td>
                            <td rowspan="2" class="rowspan" width="10"><hr></td>
                            <td width="70">
                                <div class="attribute-main">
                                    <p>{{ selectedPokemon.moves[0].move.name }}</p>
                                </div>
                            </td>
                        </tr>
                        <tr>
                            <td>
                                <div class="attribute-title">
                                    weight
                                </div>
                            </td>
                            <td>
                                <div class="attribute-title">
                                    Height
                                </div>
                            </td>
                            <td>
                                <div class="attribute-title">
                                    Moves
                                </div>
                            </td>
                        </tr>
                    </table>
                </div>
                <div class="base-stats" :style="colorStyle">
                    <p>Base Stats</p>
                </div>
                <div class="stats-section" :style="colorStyle">
                    <table width="100%" border="0" cellpadding="0" cellspacing="0">
                        <tr>
                            <th width="10">
                                <p>HP</p>
                            </th>
                            <td rowspan="6" class="rowspan" width="10"><hr></td>
                            <td width="10"><p>{{ selectedPokemon.stats[0].base_stat }}</p></td>
                            <td>
                                <div class="progress-bar">
                                    <div class="outer-bar" :style="headerStyle"></div>
                                    <div class="inside-bar" :style="hpStyles"></div>
                                </div>
                            </td>
                        </tr>
                        <tr>
                            <th width="10">
                                <p>ATK</p>
                            </th>
                            <td><p>{{ selectedPokemon.stats[1].base_stat }}</p></td>
                            <td>
                                <div class="progress-bar">
                                    <div class="outer-bar" :style="headerStyle"></div>
                                    <div class="inside-bar" :style="atkStyles"></div>
                                </div>
                            </td>
                        </tr>
                        <tr>
                            <th width="10">
                                <p>DEF</p>
                            </th>
                            <td><p>{{ selectedPokemon.stats[2].base_stat }}</p></td>
                            <td>
                                <div class="progress-bar">
                                    <div class="outer-bar" :style="headerStyle"></div>
                                    <div class="inside-bar" :style="defStyles"></div>
                                </div>
                            </td>
                        </tr>
                        <tr>
                            <th width="10">
                                <p>SATK</p>
                            </th>
                            <td><p>{{ selectedPokemon.stats[3].base_stat }}</p></td>
                            <td>
                                <div class="progress-bar">
                                    <div class="outer-bar" :style="headerStyle"></div>
                                    <div class="inside-bar" :style="satkStyles"></div>
                                </div>
                            </td>
                        </tr>
                        <tr>
                            <th width="10">
                                <p>SDEF</p>
                            </th>
                            <td><p>{{ selectedPokemon.stats[4].base_stat }}</p></td>
                            <td>
                                <div class="progress-bar">
                                    <div class="outer-bar" :style="headerStyle"></div>
                                    <div class="inside-bar" :style="sdefStyles"></div>
                                </div>
                            </td>
                        </tr>
                        <tr>
                            <th width="10">
                                <p>SPD</p>
                            </th>
                            <td><p>{{ selectedPokemon.stats[5].base_stat }}</p></td>
                            <td>
                                <div class="progress-bar">
                                    <div class="outer-bar" :style="headerStyle"></div>
                                    <div class="inside-bar" :style="spdStyles"></div>
                                </div>
                            </td>
                        </tr>
                    </table>
                </div>
            </div>
        </div>
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