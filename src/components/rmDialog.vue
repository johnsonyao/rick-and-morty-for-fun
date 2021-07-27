<template>
<div class="rmOverlay"  v-if="isVisible">
	<div class="rmOverlay__container"  @click="hideOverlay">
		
	</div>
	<div class="rmOverlay__content">
		<slot></slot>
	</div>
</div>
</template>

<script>
export default {
  name: 'rm-dialog',
  props: {
		value: {
			type: Boolean,
			required: false
		},
  },
  computed: {
	  isVisible(){
		  return this.value;
	  }
  },
  methods: {
	  hideOverlay(){
		  this.$emit('on-close');
	  }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	.rmOverlay {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		width: 100%;
		height: 100vh;
		z-index: 2;

		&__container {
			position: absolute;
			background: rgba(0,0,0,0.7);
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;
			z-index: 3;
			animation: fadein 0.5s;
		}

		&__content {
			position: absolute;
			z-index: 4;
			top: 10%;
			width: 100%;
			display: flex;
			justify-content: center;
		}

	}
	@keyframes fadein {
		from {
			opacity:0;
		}
		to {
			opacity:1;
		}
	}
</style>
