<template lang="pug">
.expirience
	p.text_font_aktrobat.expirience__title Опыт переработки:
	.expirience__text-stats
		p.text_font_roboto.expirience__text-stat Уровень {{ level }}
		p.text_font_roboto.expirience__text-stat {{ exp }} / {{ expToNextLevel }}
	ProgressBar(:cur="exp", min="0", :max="expToNextLevel")
</template>

<script>
import ProgressBar from "./ProgressBar.vue";

export default {
	components: {
		ProgressBar,
	},
	props: ["level", "exp", "expToNextLevel"],
};
</script>

<style lang="scss">
.expirience__text-stats {
	display: flex;
	justify-content: space-between;
	margin-bottom: .25vw;
}

.expirience__title {
	text-transform: uppercase;
	font-weight: 800;
	font-size: 1.75vw;
}

.expirience__text-stat {
	font-weight: 700;
	font-size: 1vw;

	color: #cfdef3;
}
</style>