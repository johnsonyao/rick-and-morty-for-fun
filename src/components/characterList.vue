<template>
	<div class="container">
		<div class="character_list"  v-if="characters.length && !isLoading">
			<rm-header @on-search="onCharaterSearch($event)"/>
			<template v-if="searchKeyword.length">
				<template v-if="searchResults.length">
					<template v-for=" character in searchResults" >
						<CharacterCard :character="character" :key="character.id" @on-card-click="getCharacterProfile($event)" />
					</template>
				</template>
				<template v-else>
					<div class="emptyMessage">
						We are unable to find the character with keyword "{{searchKeyword}}".
					</div>
				</template>
			</template>
			<template v-else>
				<template v-for=" character in characters" >
					<CharacterCard :character="character" :key="character.id" @on-card-click="getCharacterProfile($event)" />
				</template>
			</template>
			<!--- Character Profile Popup ---->
			<rm-Dialog v-model="cDialog" @on-close="cDialog = false">
				<profile-card 
					:characterData.sync="profile" 
					:episodesData.sync="episodes" 
					@on-close-profile="cDialog = false" />
			</rm-Dialog>
			<!--- Pagination --->
			<rm-pagination 
				:next="nextPage"
				:prev="prePage"
				:total="totalPage"
				:count="resultCount"
				:page="currentPage"
				@on-page-update="onPageChange($event)"
			/>
		</div>
		<div v-if="isLoading" class="container__loader">
			<rm-loader>
				Loading Characters
			</rm-loader>
		</div>
	</div>
</template>

<script>

import CharacterCard from '@/components/characterCard.vue'
import ProfileCard from '@/components/profileCard.vue'
import rmDialog from '@/components/rmDialog.vue'
import rmLoader from '@/components/rmLoader.vue'
import rmHeader from '@/components/rmHeader.vue'
import rmPagination from '@/components/rmPagination.vue'

export default {
	name: 'characterList',
	components: {
	    CharacterCard,
		ProfileCard,
		rmDialog,
		rmLoader,
		rmHeader,
		rmPagination
	},
	data: () => ({
		characters: [],
		cDialog: false,
		profile: null,
		episodes: [],
		searchKeyword: '',
		nextPage: '',
		prePage:'',
		totalPage: 0,
		resultCount: 0,
		currentPage: 1,
		isLoading: true,
		requestURL: 'https://rickandmortyapi.com/api/character/'
	}),
	created(){
		this.getCharacterList();
	},
	computed: {
		searchResults(){
			if(this.searchKeyword){
				return this.characters.filter((character)=>{
					return this.searchKeyword.toLowerCase().split(' ').every(v => character.name.toLowerCase().includes(v))
				})
			}else{
				return this.characters;
			}
		}
	},
	methods : {
		getCharacterList() {
			fetch(this.requestURL)
			.then( (response)=>{
				return response.json();
			}).then( (data) => {
					this.totalPage = data.info.pages
					this.nextPage = data.info.next;
					this.prePage = data.info.prev;
					this.resultCount = data.info.count;

					setTimeout(()=>{
						this.characters = data.results;
						this.isLoading = false;
					}, 1500);
			}).catch( (err) => {
				//error handling
				console.error(`oops, something is wrong, here's your: ${err}`);
			})
		},
		getCharacterProfile(id) {
			this.resetProfile();
			fetch(`https://rickandmortyapi.com/api/character/${id}`)
				.then( (response)=>{
					return response.json();
				}).then( (data) => {
					this.cDialog = true;
					this.episodes = data.episode.map( (episode) => {
						const tempStr = episode.indexOf('episode/');
						const newString = episode.substring(tempStr, episode.length).replace('/', ' ');
						const container = {};
						container.url = episode;
						container.name = newString;

						return container;
					});

					this.profile = data;
				}).catch( (err) => {
					//error handling
					console.error(`oops, something is wrong, here's your: ${err}`);
			})	
		},
		resetProfile(){
			this.episodes = [];
		},
		onCharaterSearch(keyword){
			this.searchKeyword = keyword;
		},
		setCurrentPage(){
			//hacky way to return the current page number, because the api didn't provide that!!
			const tempStr = this.requestURL.indexOf('page=');
			const newString = this.requestURL.substring(tempStr, this.requestURL.length).replace('page=', '');
			this.currentPage = parseInt(newString);
		},
		onPageChange(requestURL){
			this.isLoading = true;
			this.requestURL = requestURL;
			this.setCurrentPage();
			this.getCharacterList();
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	.container {
		max-width: 1200px;
		width: 100%;
		margin: 80px auto;
		height: 100vh;

		&__loader {
			width: 100%;
			display: flex;
			height: 100vh;
			justify-content: center;
			align-items: center;
		}
	}

	.emptyMessage{
		width: 100%;
		padding: 32px 0px;
		text-align: center;
		font-size: 1.2rem;
	}

	.character_list {
		display: flex;
		justify-content: flex-start;
		flex-wrap: wrap;
		width: 100%;
		padding-bottom: 120px;
	}

</style>
