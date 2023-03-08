<template lang="">

    <h1 v-if="!pokemon"> Espere por favor... </h1>

    <div v-else="pokemon">

        <h1>¿Quién es este pokémon?</h1>
      
        <PokemonPicture :pokemonId="pokemon.id" 
                        :showPokemon="showPokemon" />
   
        <PokemonOptions :pokemons="pokemonArr"
                        @selection-pokemon="checkAnswer"/>

        
        <template v-if="showAnswer">
            <button @click="newGame">
                Nuevo Juego
            </button>

        </template>
    </div>


</template>



<script>

import PokemonPicture from '@/components/PokemonPicture.vue';
import PokemonOptions from '@/components/PokemonOptions.vue';

import getPokemonsOptions from '@/helpers/getPokemonOptions'
import Swal from 'sweetalert2'


// console.log( getPokemonsOptions());

export default {
    name: 'PokemonPage',
    components: {
        PokemonPicture,
        PokemonOptions
    },

    props:{

    },

    data() {
        return {
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            msg: '',
            resetGame: false,
            counter: null,
        }
    },

    methods: {
        async mixPokemonArr(){
            this.pokemonArr = await getPokemonsOptions();

            const rndInt = Math.floor( Math.random() * 4 );
            
            this.pokemon = this.pokemonArr[rndInt];

            console.log(this.pokemonArr);
        },
        checkAnswer(pokemonId){
            this.showPokemon = true;
            this.showAnswer = true;

            if(pokemonId === this.pokemon.id){
               
                Swal.fire({
                    position: 'center',
                    icon: 'success',
                    title: this.msg = `Correcto, es ${this.pokemon.name}`,
                    showConfirmButton: false,
                    timer: 2500
            })
                
            } else{
                Swal.fire({
                    position: 'center',
                    icon: 'error',
                    title: this.msg = `Error, era ${this.pokemon.name}`,
                    showConfirmButton: false,
                    timer: 2500
            })
                
            }

            
        },
        newGame(){
           this.mixPokemonArr();
           this.showPokemon = false;
           this.showAnswer = false;
           this.pokemon = null;
           this.pokemonArr = [];
        }
    },
    mounted(){
        this.mixPokemonArr()
    }
}
</script>

