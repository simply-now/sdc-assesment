<template>
    <div class="py-3 px-5 has-background-sdc-grey has-border-bottom">
      <div class="container">
        <section class="section sdc-section">
        <p class="subtitle">
                Pokemon Cards
        </p>
        <h1 class="title">Gotta catch ‘em all</h1>
        <div class="columns">
            <div class="column" v-for='(miniCardList, index) in pokemonMiniCardList'>
                <div class="card sdc-card" v-for='(item, index) in miniCardList'>
                    <div class="card-image">
                        <figure class="image is-4by3">
                            <img :src='item.image' :alt='item.name'>
                        </figure>
                    </div>
                    <div class="card-content">
                        <div class="title is-4">{{item.name}}</div>
                        <div class="pokemon-abilities">
                            <strong>Abilities:</strong>
                            <span v-if='item.abilities.length <= 1'>
                                {{item.abilities.ability.name}}
                            </span>
                            <span v-else>
                                <span v-for='(ability, index) in item.abilities'>
                                    {{ability.ability.name}}<span v-if="index != item.abilities.length - 1">, </span>
                                </span>
                            </span>
                        </div>
                        <div class="pokemon-types">
                            <strong>Types:</strong>
                            <span v-if='item.types.length <= 1'>
                                <span v-for='(type, index) in item.types'>
                                    {{type.type.name}}<span v-if="index != item.types.length - 1">, </span>
                                </span>
                            </span>
                            <span v-else>
                                <span v-for='(type, index) in item.types'>
                                    {{type.type.name}}<span v-if="index != item.types.length - 1">, </span>
                                </span>
                            </span>
                        </div>
                        <!-- using {{item.stats}} -->
                        <div class="pokemon-hp">
                            <strong>HP: </strong><span>{{item.stats[0].base_stat}}</span>
                        </div>
                        <div class="pokemon-attack">
                            <strong>Attack: </strong><span>{{item.stats[1].base_stat}}</span>
                        </div>
                        <div class="pokemon-defense">
                            <strong>Defense: </strong><span>{{item.stats[2].base_stat}}</span>
                        </div>
                    </div>
                    <footer class="card-footer">
                        <a class="button is-primary is-outlined" :href=' "https://pokemon.fandom.com/wiki/" + item.name' >Learn more</a>
                    </footer>
                </div>
            </div>
        </div>
        </section>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'PokemonCards',
    props: {
        pokemonCardList: {
            type: Array,
            required: true
        }
    },
    data () {
        return {
            pokemonCardDataList: ["Loading..."],
            pokemonMiniCardList: ["Loading..."],
            wikiUrl: ""
        }
    },
    created () {
    if (this.pokemonCardList && this.pokemonCardDataList && this.pokemonMiniCardList) {
        this.pokemonCardDataList = [];
        this.pokemonMiniCardList = [];
        console.log(this.pokemonCardList)
        let TempPokemonCardList = [...this.pokemonCardList]
        console.log(TempPokemonCardList)
        TempPokemonCardList.forEach((element, index) => {
        console.log(element);
        let pokemonName = element.toLowerCase()
        let pokemonNameUrl = "https://pokeapi.co/api/v2/pokemon/" + pokemonName
        this.wikiUrl = "https://pokemon.fandom.com/wiki/" + pokemonName
        console.log("Request:"+ pokemonNameUrl)
        let requestOptions = {method: 'GET', redirect: 'follow'}
        fetch(pokemonNameUrl, requestOptions)
            .then(response => response.text())
            .then(result => {
                let data = JSON.parse(result);
                let image = data.sprites.other['official-artwork'].front_default
                let abilities = data.abilities
                let types = data.types
                let stats = data.stats
                let miniCardList = [{'name':pokemonName, 'image': image, 'abilities': abilities, 'types':types, 'stats':stats}]
                this.pokemonMiniCardList.push(miniCardList)
                console.log(miniCardList)

                this.pokemonCardDataList.push(result)
                }
                )
            .catch(error => console.log('error', error));
        });
    } else if (this.pokemonCardDataList) {
        this.pokemonCardDataList = ["Error Pokémon List Not found"]
    }
    }
  }
  </script>
  