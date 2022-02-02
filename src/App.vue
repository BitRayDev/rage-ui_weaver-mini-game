<template>
	<WeaverMiniGame />
</template>

<script>
import WeaverMiniGame from "./components/WeaverMiniGame.vue";

export default {
	name: "App",
	components: {
		WeaverMiniGame,
	},
};
</script>

<style lang="scss">
* {
    margin: 0;
    padding: 0;
}

.text_font_aktrobat {
	font-family: "Akrobat";
	letter-spacing: 0.03em;
}

.text_font_roboto {
	font-family: "Roboto";
}

</style>
