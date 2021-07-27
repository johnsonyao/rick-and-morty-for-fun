<template>
	<div class="card" v-if="profile"   @click="$emit('on-card-click', profile.id )">
		<div class="card__image" :style="`background-image:url('${profile.image}')`">
		</div>
		<div class="card__title">
			{{profile.name}}
		</div>
	</div>
</template>

<script>

export default {
  	name: 'characterCard',
	props: {
		character: {
			type: Object,
			required: true,
		}
	},
	components: {
		
	},
	created(){
		this.profile = this.character;
	},
	data: () => ({
		profile: null
	})
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

	@keyframes zoom-in-zoom-out {
		0% {
			transform: scale(1, 1);
		}
		50% {
			transform: scale(1.1, 1.1);
		}
		100% {
			transform: scale(1, 1);
		}
	}
	
	@keyframes highlight-link {
		0% {
			font-weight: 400;
		}
		100% {
			font-weight: 600;
		}
	}
	.card {
		max-width: 20%;
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		margin:24px 0px;


		@media only screen and (max-width: 960px) {
			max-width: 25%;
		}

		@media only screen and (max-width: 800px) {
			max-width: 33.3%;
		}
		@media only screen and (max-width: 596px) {
			max-width: 50%;
		}
		@media only screen and (max-width: 480px) {
			max-width: 100%;
		}

		&__image {
			border-bottom: 1px solid #ddd;
			border-radius: 50%;
			overflow: hidden;
			margin: 24px;
			width: 180px;
			height: 180px;
			background-position: center center;

			&:hover {
				animation: zoom-in-zoom-out 1s ease-out;
			}
		}

		&__title {
			font-size: 1.3rem;
			font-weight: 400;

			&:hover {
				color: #526e2d;
				cursor: pointer;
				font-weight: 600;
				animation: highlight-link 0.5s;
			}
		}
	}
</style>
