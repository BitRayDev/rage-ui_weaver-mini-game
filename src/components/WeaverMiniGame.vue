<template lang="pug">
.layout
	.dashboard
		QuitButton(text="Выход")

		.dashboard__stats
			h1.text_font_aktrobat.dashboard__title Производство ткани

			.dashboard__options
				DashboardOption(title="Заработная плата:", :value="`${paymentPerKilo} $ / кг`")
				DashboardOption(title="Переработано:", :value="`${alreadyRecycled} кг`")
				DashboardOption(title="Вы заработали:", :value="`${paymentSummary} $`")

			.dashboard__expirience
				Expirience(:level="level", :exp="exp", :expToNextLevel="expToNextLevel")

	.machine(@mousedown="startDrag", ref="machineContainer")
		include ../assets/img/weaver-mini-game/machine.svg
		transition(name="fade")
			Popup.machine__popup(v-if="isPopupShowed", :type="popupType", :text="popupText")

	.info
		Hint(
			text="Чтобы начать процесс переработки хлопка, перетяните нити с помощью левой кнопки мыши в гнездо таким образом, чтобы все три нити пересекались и запустите станок."
		)
		EngineButton.info__button(@click="startEngine", text="Запуск станка")
</template>

<script>
import ProgressBar from "./ProgressBar.vue";
import QuitButton from "./QuitButton.vue";
import DashboardOption from "./DashboardOption.vue";
import Expirience from "./Expirience.vue";
import Hint from "./Hint.vue";
import EngineButton from "./EngineButton.vue";
import Popup from "./Popup.vue";


export default {
	components: {
		QuitButton,
		DashboardOption,
		Expirience,
		Hint,
		EngineButton,
		Popup,
	},
	data: function () {
		return {
			paymentPerKilo: 20,
			alreadyRecycled: 124,
			level: 1,
			exp: 5,
			expToNextLevel: 10,

			machineSvg: undefined,
			machineSvgViewbox: undefined,
			machineSvgRect: undefined,

			curLine: undefined,
			curLineId: -1,

			linesConnected: [false, false, false],

			isPopupShowed: false,
			popupText: "Операция завершена упешно!",
			popupType: "success"
		};
	},
	mounted: function () {
		this.machineSvg = document.getElementById(`machine-svg`);
		this.machineSvgViewbox = this.machineSvg.viewBox.baseVal;
		this.machineSvgRect = this.machineSvg.getBoundingClientRect();

		this.linesConnected.forEach((line, index) => this.resetLine(index + 1));
	},
	methods: {
		startDrag: function (e) {
			if (e.target.id.includes("output")) {
				const id = Number.parseInt(e.target.id.match(/\d+/g)[0]);

				if (this.linesConnected[id - 1]) return;

				this.curLine = this.machineSvg.getElementById(`line${id}`);
				this.curLineId = id;

				this.machineSvg.onmouseover = this.tryToConnect;
				document.onmousemove = this.handleDrag;
				document.onmouseup = this.stopDrag;
			}
		},
		handleDrag: function (e) {
			this.moveEndPointAt(e.clientX, e.clientY);
		},
		moveEndPointAt: function (x, y) {
			const calibratedX = ((x - this.machineSvgRect.left) * this.machineSvgViewbox.width) / this.machineSvgRect.width;
			const calibratedY = ((y - this.machineSvgRect.top) * this.machineSvgViewbox.height) / this.machineSvgRect.height;
			this.curLine.setAttribute("x2", calibratedX);
			this.curLine.setAttribute("y2", calibratedY);
		},
		stopDrag: function () {
			if (!this.linesConnected[this.curLineId - 1]) this.resetLine(this.curLineId);

			this.resetCallbacks();
		},
		resetCallbacks: function () {
			document.onmousemove = null;
			document.onmouseup = null;
			this.machineSvg.onmouseover = null;
		},
		resetLine: function (lineId) {
			const line = this.machineSvg.getElementById(`line${lineId}`);
			const output = this.machineSvg.getElementById(`output${lineId}`);
			const outputRect = output.getBoundingClientRect();
			const x = outputRect.x + outputRect.width / 2;
			const y = outputRect.y + outputRect.height / 2;

			const calibratedX = ((x - this.machineSvgRect.left) * this.machineSvgViewbox.width) / this.machineSvgRect.width;
			const calibratedY = ((y - this.machineSvgRect.top) * this.machineSvgViewbox.height) / this.machineSvgRect.height;

			line.setAttribute("x1", calibratedX);
			line.setAttribute("y1", calibratedY);
			line.setAttribute("x2", calibratedX);
			line.setAttribute("y2", calibratedY);

			this.linesConnected[lineId - 1] = false;
			const light = this.machineSvg.getElementById(`light${4 - lineId}`);
			light.classList.remove("light_active");
		},
		tryToConnect: function (e) {
			if (e.target.id.includes("input")) {
				const inputId = Number.parseInt(e.target.id.match(/\d+/g)[0]);
				if (this.curLineId + inputId === 4) {
					const inputRect = e.target.getBoundingClientRect();
					const x = inputRect.x + inputRect.width / 2;
					const y = inputRect.y + inputRect.height / 2;

					this.moveEndPointAt(x, y);

					this.linesConnected[this.curLineId - 1] = true;
					const light = this.machineSvg.getElementById(`light${4 - this.curLineId}`);
					light.classList.add("light_active");

					this.resetCallbacks();
				}
			}
		},
		startEngine: function () {
			if (this.linesConnected.every((isLineConnected) => isLineConnected)) {
				// TODO: ТУТ ЛОГИКА ОБРАБОТКИ

				this.popupType = "success";
				this.popupText = "Операция завершена успешно";

				for (let i = 1; i <= 3; i++) {
					this.resetLine(i);
				}
			} else {
				this.popupType = "error";
				this.popupText = "Не все нити заправлены";
			}

			this.isPopupShowed = true;
			setTimeout(() => {
				this.isPopupShowed = false;
			}, 2500);
		},
	},
	computed: {
		paymentSummary: function () {
			return this.paymentPerKilo * this.alreadyRecycled;
		},
	},
};
</script>

<style lang="scss">
.layout {
	display: grid;

	width: 100vw;
	height: 100vh;

	background: center/cover url("../assets/img/weaver-mini-game/bg.png");

	grid-template-columns: 35% 1fr 25%;
}
* {
	color: white;
}

.dashboard {
	display: flex;

	padding: 3.5vw;
	padding-right: 0;

	flex-direction: column;
	align-items: flex-start;

	gap: 1vw;
}

.dashboard__title {
	font-weight: 700;
	font-size: 3vw;
}

.dashboard__stats {
	display: flex;

	flex-grow: 1;

	flex-direction: column;
	justify-content: space-between;
}

.dashboard__options {
	display: flex;
	flex-direction: column;
	gap: 2vw;
}

.machine {
	height: 100vh;

	display: flex;
	justify-content: center;

	position: relative;
}

.machine__popup {
	position: absolute;
	top: 47%;
	left: 50%;

	transform: translate(-50%, -50%);

	transition: 1s;
}

.info {
	padding: 3.5vw;
	padding-left: 0;

	display: flex;
	flex-direction: column;
	justify-content: flex-end;
}

.info__button {
	width: fit-content;
}

.line {
	filter: drop-shadow(0 0.5vw 0.1vw rgba(0, 0, 0, 0.32));
}

.light {
	fill: #ffc700;
	filter: drop-shadow(0 0 1vw #ff7a00);
}

.light_active {
	fill: #99f700;
	filter: drop-shadow(0 0 1vw #99f600);
}

.fade-enter-active,
.fade-leave-active {
	transition: opacity 0.5s;
}
.fade-enter-from,
.fade-leave-to {
	opacity: 0;
}
</style>