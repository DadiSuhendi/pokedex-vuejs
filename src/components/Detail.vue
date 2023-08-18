<script>
    export default {
        props: {
            headerStyle : Object,
            showHome : Function,
            previousPokemon: Function,
            nextPokemon:Function,
            getTypeColor :Function,
            colorStyle: Object,
            hpStyles: Object,
            atkStyles: Object,
            defStyles: Object,
            satkStyles: Object,
            sdefStyles: Object,
            spdStyles: Object,
            selectedPokemon : [String, Object],
        },
    }
</script>

<template>
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
            <img v-if="selectedPokemon" :src="selectedPokemon.sprites.other['official-artwork'].front_default" alt="" width="200">
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
                                <p v-if="selectedPokemon.moves">{{ selectedPokemon.moves[0].move.name }}</p>
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
                        <td width="10"><p v-if="selectedPokemon.stats">{{ selectedPokemon.stats[0].base_stat }}</p></td>
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
                        <td><p v-if="selectedPokemon.stats">{{ selectedPokemon.stats[1].base_stat }}</p></td>
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
                        <td><p v-if="selectedPokemon.stats">{{ selectedPokemon.stats[2].base_stat }}</p></td>
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
                        <td><p v-if="selectedPokemon.stats">{{ selectedPokemon.stats[3].base_stat }}</p></td>
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
                        <td><p v-if="selectedPokemon.stats">{{ selectedPokemon.stats[4].base_stat }}</p></td>
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
                        <td><p v-if="selectedPokemon.stats">{{ selectedPokemon.stats[5].base_stat }}</p></td>
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
</template>
