<template>
    <div>
        <h1>Inicio</h1>
        <b-container>
            <b-row>
                <b-col cols="4" v-for="(pokemon, iterator) in pokemons" :key="iterator">
                    <b-card
                        :title="pokemon.name"
                        :img-src="'https://picsum.photos/id/' + (iterator + 50) + '/100/100'"

                        img-alt="Imágen"
                        style="max-with: 20rem;">
                        <!--                        <p class="card-text">-->
                        <!--                            tipo-->
                        <!--                            {{ pokemon }}-->
                        <!--                        </p>-->
                        <b-button variant="primary" @click="getPokemonDetail(pokemon.url)">Ver mas...</b-button>
                    </b-card>
                </b-col>
            </b-row>
        </b-container>
        <b-modal id="modal-1" title="HABILIDADES">
            <div class="ability" v-if="pokemodal.abilities.length">
                <p class="my-4" v-for="(ability, iterator) in pokemodal.abilities" :key="iterator">Habilidad:
                    {{ ability.ability.name }}</p>
            </div>
        </b-modal>
    </div>

</template>

<script>

import axios from 'axios'
//axios.defaults.headers.common['user-key'] = '7y9uadbf8z83il0wu2bmct3dapum4u'
axios.defaults.headers.common['x-Requested-With'] = 'XMLHttpRequest'

export default {
    data() {
        return {
            pokemons: [], //aqui guardas los pokemon que te vas a traer
            pokemodal: {
                abilities: []
            },
        }
    },
    async created() { //esto se hace cuando se crea la pagina
        let res = await axios.get('https://pokeapi.co/api/v2/pokemon')
        console.log('created')
        console.log(res)
    },
    async asyncData() {  //aqui todo el tema del ajax con axios esto se hace antes de renderizarse la página
        //const cors = 'https://cors-anywhere.herokuapp.com/corsdemo'
        let res = await axios.get('https://pokeapi.co/api/v2/pokemon')
        console.log(res)
        return {
            pokemons: res.data.results //guardo en pokemon todos los datos de la petición
        }
    },
    methods: {
        getPokemonDetail(url) {
            axios.get(url).then(response => {
                console.log(response.data)
                this.pokemodal = response.data;
                this.$bvModal.show('modal-1');
            })

        }
    }
}
</script>

