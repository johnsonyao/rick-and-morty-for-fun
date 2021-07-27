<template>
	<div class="profileCard" v-if="profile">
		<div class="profileCard__header">
			{{profile.name}}
		</div>
		<div class="profileCard__content">
			<div class="profileCard__image">
				<img :src="profile.image" />
			</div>
			<div class="profileCard__detail">
				<div class="info">
					<div class="info__label">
						Name:
					</div>
					<div class="info__data">
						{{profile.name}}
					</div>
				</div>
				<div class="info">
					<div class="info__label">
						Status:
					</div>
					<div class="info__data">
						{{profile.status}}
					</div>
				</div>
				<div class="info">
					<div class="info__label">
						Type:
					</div>
					<div class="info__data">
						{{profile.type | emptyData}}
					</div>
				</div>
				<div class="info">
					<div class="info__label">
						Gender:
					</div>
					<div class="info__data">
						{{profile.gender | emptyData}}
					</div>
				</div>
			</div>
		</div>
		<div class="profileCard__episodes">
			<div @click="showEpisodes" class="episode_toggler">
				<template v-if="toggleEpisodeList">
					Hide Episodes
				</template>
				<template v-else>
					Show Episodes
				</template>
			</div>
			<div v-if="toggleEpisodeList && episodes.length" class="episode_container">
				<div v-for="episode in episodes" :key="episode.name" class="episode_block" @click="getEpisode(episode.url)">
					{{episode.name}}
				</div>
			</div>
		</div>
		<div class="profileCard__actions">
			<rm-button @click="$emit('on-close-profile')">Close</rm-button>
		</div>			
	</div>
</template>

<script>

import rmButton from '@/components/rmButton.vue'
export default {
  	name: 'profileCard',
	props: {
		characterData: {
			type: Object,
			required: true,
		},
		episodesData: {
			type: Array,
			required: false
		}
	},
	components: {
		rmButton
	},
	created(){
		this.profile = this.characterData;
		this.episodes = this.episodesData;
	},
	filters: {
		emptyData(value){
			return value.trim().length ? value : 'N/A';
		}
	},
	data: () => ({
		profile: null,
		episodes: [],
		toggleEpisodeList: false

	}),
	methods: {

		showEpisodes(){
			this.toggleEpisodeList = !this.toggleEpisodeList;
		},
		getEpisode(url){
			console.log(`This is the URL to make further request ${url}`);
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

	.profileCard {
		background-color: #fff;
		max-width: 430px;
		width: 100%;
		box-shadow: 1px 3px 4px rgba(0,0,0,0.3);
		border-radius: 6px;
		margin: 0 12px;

		&__header {
			border-bottom: 1px solid #ddd;
			padding:12px 24px;
			font-size: 1.2rem;
		}

		&__content {
			padding:12px 12px;
			display: flex;
			justify-content: space-between;
			flex-wrap: wrap;
		}

		&__image {
			width: 140px;
			img{
				width: 100%;
				border-radius: 5px;
			}

			@media only screen and (max-width: 596px) {
				width: 80px;
			}
		}

		&__detail {
			width: calc( 100% - 148px );
			display: flex;
			flex-direction: column;

			@media only screen and (max-width: 596px) {
				width: calc( 100% - 88px );
			}
			.info {
				display: flex;
				padding: 2px 0px;
				
				&__label {
					font-weight: 600;
					margin-left: 6px;
					min-width: 80px;
					text-align: left;
					font-size: 0.9rem;
				}

				&__data {
					text-align: left;
					font-size: 0.9rem;
				}
			}
		}

		&__actions {
			padding: 12px 24px;
			border-top: 1px solid #ddd;
			display: flex;
			justify-content: flex-end;
		}

		&__episodes {
			padding: 8px;
		}
	}
	.episode_toggler {
		width: 100%;
		text-align: center;
		padding: 12px 0px;
		font-weight: 600;
		color: #44281d;
		font-size: 0.85rem;

		&:hover {
			cursor: pointer;
			background-color: rgba(228, 167, 136, 0.2);
		}
	}

	.episode_container {
		display: flex;
		justify-content: flex-start;
		flex-wrap: wrap;
		padding: 6px;

		@media only screen and (max-width: 596px) {
			overflow-y: auto;
			max-height: 180px;
		}

		.episode_block {
			padding:8px 12px;
			border: 1px solid #ccc;
			border-radius: 2px;
			background-color: #f5f5f5;
			text-transform: capitalize;
			cursor: pointer;
			font-size: 0.75rem;
			font-weight: 600;
			width: 70px;
			margin: 2px;

			&:hover {
				background-color: #44281d;
				border: 1px solid #44281d;
				color: #fff;
			}
		}
	}
</style>
