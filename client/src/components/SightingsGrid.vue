<template lang="html">
	<div id="sightingsGrid">
		<div class="sighting" v-for="(sighting,index) in sightings">
			<h2>{{ sighting.species }}</h2>
			<p>{{ sighting.location }} on {{ sighting.date|format }}</p>

			<button v-on:click="deleteASighting(sighting._id,index)">Delete Sighting</button>
		</div>
	</div>
</template>

<script>
import { eventBus } from '../main';
import SightingsService from '../services/SightingService.js'
export default {
	name: "sightings-grid",
	filters: {
		format(value){
			return new Date(value).toLocaleString().substring(0, 10);
		}
	},
	data(){
		return {
			sightings: []
		}
	},
	mounted() {
		SightingsService.getSightings()
		.then(sighting => this.sightings = sighting);

		eventBus.$on('sighting-added', (data) =>
		this.sightings.push(data));
	},
	methods: {
		deleteASighting(id, index){
			SightingsService.deleteSighting(id);
			this.sightings.splice(index,1);
		}
	}

}
</script>

<style lang="css" scoped>
#sightingsGrid {
	display: flex;
	flex-wrap: wrap;
	justify-content: space-evenly;
}

h2 {
	padding: 0;
	margin: 0;
}

.sighting {
	width: 30%;
	background: rgba(255, 255, 255, 0.7);
	margin-bottom: 20px;
	padding: 25px;
}

button {
	color: #fff;
	border: none;
	font-size: 18px;
	padding: 10px;
	margin-top: 10px;
	background: #F55536;
}
</style>
