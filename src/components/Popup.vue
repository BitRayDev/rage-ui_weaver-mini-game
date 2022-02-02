<template lang="pug">
.popup(:class="{ 'popup_type_success': type == 'success', 'popup_type_error': type == 'error' }")
	img.popup__icon(:src="iconUrl")
	span.popup__text.text_font_roboto {{ text }}
</template>

<script>
import successIcon from "../assets/img/weaver-mini-game/icons/success.svg";
import errorIcon from "../assets/img/weaver-mini-game/icons/error.svg";

export default {
	props: ["type", "text"],
	computed: {
		iconUrl: function() {
			return this.type == "success" ? successIcon : errorIcon; 
		}
	}
};
</script>

<style lang="scss">
.popup {
	display: flex;
	align-items: center;

	width: fit-content;

	
	border-radius: 1vw;

	padding: 0.5vw 1vw;
}

.popup__icon {
	height: 1vw;

	margin-right: 0.5vw;
}

.popup_type_success {
	background: linear-gradient(269.35deg, #218575 0%, #3d855e 100%);
	box-shadow: 0px 0px 5px #3d855e;
}
.popup_type_error {
	background: linear-gradient(269.35deg, #A70505 0%, #790101 100%);;
	box-shadow: 0px 0px 5px #A70505;
}
</style>