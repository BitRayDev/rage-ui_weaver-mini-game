<template lang="pug">
button.engine-button.text_font_roboto {{ text }}
</template>

<script>
export default {
	props: ["text"],
};
</script>

<style lang="scss">
.engine-button {
	padding: 1vw 2vw;

	border: none;
	border-radius: .5vw;

	background: linear-gradient(158.63deg, #4776e6 -22.72%, #8763ef 81.92%);
	filter: drop-shadow(0 0.5vw 1vw rgba(110, 129, 191, 0.32));

	font-weight: bold;
	font-size: 1vw;
}
</style>