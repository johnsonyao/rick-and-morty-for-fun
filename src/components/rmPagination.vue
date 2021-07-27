<template>
    <div class="pagination">
		<div class="container">
			<div class="pagination_info">
				Page: {{page}}
			</div>

			<div class="pagination_actions">
				<div v-if="prev" class="pager" @click="onPageUpdate(prev)">
					Previous
				</div>
				<div class="pager_goto">
					<select v-model="currentPage" @change="onPageChange(currentPage)">
						<option  v-for="pager in returnPageArray" :key="pager">
							{{pager}}
						</option>
					</select>
				</div>
				<div  v-if="next" class="pager" @click="onPageUpdate(next)">
					Next
				</div>
			</div>

		</div>
    </div>
</template>

<script>
export default {
	name: 'rm-pagination',
	data: () => ({
		currentPage: 2
	}),
	props: {
		next: {
			required: true,
		},
		prev: {
			required: true,
		},
		total: {
			type: Number,
			required: true,	
		},
		count: {
			type: Number,
			required: true,	
		},
		page: {
			type: Number,
			required: true
		}
	},
	created(){
		this.currentPage = this.page;
	},
	computed:{
		returnPageArray(){
			let tempPageArray = [];

			for(let i=this.total; i>=1; i--){
				tempPageArray.push(i);
			}

			return tempPageArray;
		}
	},
	methods: {
		onPageUpdate(value){
			this.$emit('on-page-update', value)
		},
		onPageChange(value){
			const str = `https://rickandmortyapi.com/api/character/?page=${value}`;
			this.$emit('on-page-update', str)
		}

	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.pagination {
	background-color: #f0e14a;
	position: fixed;
	bottom: 0;
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
		justify-content: space-between;
		align-items: center;
	}
	.pagination_info{
		font-size: 0.9rem;
	}

	.pagination_actions{
		width: 220px;
		display: flex;
		justify-content: flex-end;
		align-items: center;

		.pager{
			border: 1px solid #44281d;
			color: #44281d;
			padding: 8px 8px;
			width: 60px;
			text-align: center;
			font-size: 0.8rem;

			&:hover {
				background-color: #44281d;
				color: #f0e14a;
				cursor: pointer;
			}
		}

		.pager_goto{
			margin: 0 3px;
			select {
				padding: 8px 8px;
				border: 1px solid #44281d;
				background-color: #f0e14a;
			}
		}
	}
}
</style>
