<template lang="pug">
.dashboard-option 
	p.text_font_aktrobat.dashboard-option__title {{ title }}
	p.text_font_roboto.dashboard-option__value {{ value }}
</template>

<script>
export default {
	props: ["title", "value"],
};
</script>

<style lang="scss">
.dashboard-option__title {
	font-weight: 800;
	font-size: 1.75vw;
}

.dashboard-option__value {
	font-weight: 700;
	font-size: 1vw;
}
</style>