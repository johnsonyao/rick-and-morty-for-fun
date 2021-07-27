<template>
    <div class="header">
		<div class="container">
			<input type="text" class="searchBar" v-model="keyword" placeholder="Search character name"  @input="onSearch" />
			<template v-if="keyword.length">
				<div class="clear" @click="onClear">Clear</div>
			</template>
		</div>
    </div>
</template>

<script>
export default {
  	name: 'rm-header',
	data: () => ({
		keyword: ''
	}),
	methods: {
		onSearch(){
			this.$emit('on-search', this.keyword);
		},
		onClear(){
			this.keyword = '';
			this.$emit('on-search', this.keyword);
		}
	}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.header {
	background-color: #44281d;
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	padding: 8px;
	z-index: 12;
	height: 48px;
	display: flex;
	justify-content: center;

	.container {
		max-width: 1200px;
		width: 100%;
		display: flex;
		position: relative;
		align-items: center;

		.searchBar {
			padding: 12px 16px 12px 40px;
			border: 1px solid #44281d;
			border-radius: 3px;
			width: 100%;
			color: #44281d;
			outline: none !important;
			font-size: 1.2rem;
		}
		.clear{
			position: absolute;
			z-index: 2;
			right: 21px;
			cursor: pointer;
		}
	}
}
</style>
