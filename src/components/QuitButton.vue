<template lang="pug">
button.quit-button
	img.quit-button__icon(src="@/assets/img/weaver-mini-game/icons/back.svg")
	p.quit-button__text.text_font_roboto {{ text }}
</template>

<script>
export default {
	props: ["iconUrl", "text"],
};
</script>

<style lang="scss">
.quit-button {
	background: none;
	border: none;
	text-align: center;
}

.quit-button__icon {
	margin-bottom: 0.5vw;
	height: .75vw;
}

.quit-button__text {
	font-size: .75vw;
	font-weight: 700;
}
</style>