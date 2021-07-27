<template>
	<button type="button"
		:class="type"
		:disabled="disabled"
		@click="onBtnClick"
	>
		<slot></slot>
	</button>
</template>

<script>
export default {
  name: 'rm-Button',
  props: {
		type: {
			type: String,
			required: false
		},
		disabled: {
			type: Boolean,
			required: false
		}
  },
  methods: {
	  onBtnClick(e) {
		  this.$emit('click');
	  }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	button {
		padding: 10px 20px;
		border: 1px solid #ddd;
		color: #333;
		background-color:#fff;
		border-radius: 4px;
		font-size: 14px;
		cursor: pointer;

		&[disabled]{
			cursor: not-allowed;
		}
		&.danger {
			background-color: #e89ac7;
			color: #fff;
		}
		&.success {
			background-color: #97ce4c;
			color: #fff;
		}
		&.info {
			background-color: #e4a788;
			color: #fff;
		}
	}
</style>
