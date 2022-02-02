<template lang="pug">

.progress-bar
	.progress-bar__fill(:style="{ width: `${fillPercent}%` }")

</template>

<script>
export default {
	props: ["cur", "min", "max"],
	computed: {
		fillPercent: function() {
			return (this.cur-this.min)/(this.max-this.min)*100
		}
	}
};
</script>

<style lang="scss">
	.progress-bar {
		height: 1vw; 

		background-color: rgba($color: #000000, $alpha: .15);
		border-radius: 1vw;
	}

	.progress-bar__fill {
		height: 100%;

		background: linear-gradient(269.64deg, #FFEB36 -33.48%, #00BEA0 132.42%);
		border-radius: 1vw;

		box-shadow: 0 0 .5vw #00BEA0;
	}
</style>