<template lang="pug">
.hint
	img.hint__icon(src="@/assets/img/weaver-mini-game/icons/info.svg")
	p.hint__text.text_font_roboto {{ text }}
</template>

<script>
export default {
	props: ["text"],
};
</script>

<style lang="scss">
.hint {
	width: 20vw;
	margin-bottom: 0.5vw;
}

.hint__icon {
	width: 1.75vw;

	margin-bottom: 0.5vw;
}

.hint__text {
	font-size: 1vw;
	font-weight: 500;

	line-height: 150%;
}
</style>