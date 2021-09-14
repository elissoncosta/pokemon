<template>
	<q-page class="flex flex-center">
		<div class="column items-center">
			<h2>{{ name }}</h2>
			<q-img :src="url" width="250px" />
		</div>
		<div class="row justify-around full-width">
			<q-input filled v-model="search" label="Digite o Nome do Pokemon" />
			<q-btn color="purple" label="Pesquisar" @click="getPokemon"/>
		</div>
		<div class="row justify-between absolute full-width container-arrows">
			<q-icon name="far fa-arrow-alt-circle-left"  color="black" class="q-ml-sm cursor-pointer" size="50px" @click="getPokemon(id - 1)">
				<q-tooltip>Anterior</q-tooltip>			
			</q-icon>

			<q-icon name="far fa-arrow-alt-circle-right" color="black" class="q-mr-sm cursor-pointer" size="50px" @click="getPokemon(id + 1)">
				<q-tooltip>Próximo</q-tooltip>
			</q-icon>
		</div>	
				
	</q-page>
</template>

<script>
import api from "../services/api";

	export default{
	name: 'PageIndex',

	data(){
		return {
			name: "",
			url: null,
			search: "ditto",
			id: "",
		};
	},
	/*Vai realizar uma requisição ao Site onde tem as informações dos pokemons*/
	async beforeMount() {
		await this.getPokemon();
	},

	methods:{
		getPokemon(id) {
			/*- api = vai pegar o endereço setado no api.js
			- Removido o function, para poder usar o this*/
			api.get(id > 0 ? '/pokemon/' + id + '/' : '/pokemon/' + this.search + '/')
			   .then((response) => {
				// handle success
				this.id  = response.data.id;
				this.name = response.data.name;
				this.search = response.data.name;
				this.habilidade = response.data.habilidade
				this.url  = response.data.sprites.other.dream_world.front_default;
			})
			.catch((error) => {
				// handle error
				this.triggerNegative();
			})
			.then(() => {
				// always executed
			});
		},
		triggerPositive () {
			this.$q.notify({
			type: 'positive',
			position: 'top',
			message: 'Pokemon Encontrado !.'
			})
		},

		triggerNegative () {
			this.$q.notify({
			type: 'negative',
			position: 'top',
			message: 'Houve um problema com durante o Processo ! .'
			})
		},
	},
};
</script>

<style lang="scss" scoped>
.container-arrows {

}

</style>