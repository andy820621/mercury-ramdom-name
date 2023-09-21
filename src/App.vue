<template>
	<div class="main-content">
		<div class="img-box">
			<img class="main-img" src="./assets/background-for-name.png" alt="" />
			<div class="current-number">
				<p v-if="alertMessage">{{ alertMessage }}</p>
				<ul v-else class="numberArray">
					<li
						v-for="(number, index) in randomNumbers"
						:key="index"
						:style="{ '--num': liFontSize }"
					>
						{{ stringArray[number - 1] }}
					</li>
				</ul>
			</div>
			<div class="btn" @click.prevent="generateRandomNumbers">
				<img src="./assets/logoButton.png" alt="" />
			</div>
			<div class="inputBox">
				<label for="count">需要產出的數字個數: </label>
				<input id="count" type="number" v-model="count" max="5" />
			</div>

			<button class="restart" @click="confirmRestart">重新開始</button>
		</div>
	</div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted, computed } from "vue";
const startNumber = ref(1);
const endNumber = ref(41);
const numberArray = ref<number[]>([]);
const stringArray = ref<string[]>([
	"廖裕德Land",
	"吳義信Nobu",
	"張添美Insurance",
	"何宗雄Leica",
	"林功瑞Success",
	"蔡富得Screen",
	"葉德發Building",
	"李詩峰Sticker",
	"邱慶宗Fan",
	"張育榮Jily",
	"林繼鐘John",
	"商國松Sun",
	"吳竹林Safe",
	"簡榮坤Gary",
	"劉鎮洲Marker",
	"林拴O.B.S.",
	"張振坪Wireless",
	"吳俊德David",
	"鄭明銓Bruce",
	"賴明福C.P.A.",
	"陳金鐘James",
	"許誌宏Paul",
	"黃國旺Label",
	"賴英弘Leo",
	"巫忠勇Joe",
	"陳尚彬Ocean",
	"周啟文Kevin",
	"陳浚宏Jun",
	"官彥均I-Guard",
	"吳宗憲Archie",
	"元立昇Alex",
	"朝陽Mercury",
	"林進照Mickey",
	"王文杰Jack",
	"吳輝平Eric",
	"吳贊吉Andy",
	"邱浤瑋 Brian",
	"林政豪Howard",
	"鄭加新Renew",
	"徐賢修Foster",
	"葉紡",
]);
for (let i = startNumber.value; i <= endNumber.value; i++) {
	numberArray.value.push(i);
}

const count = ref(1);
const randomNumbers = ref<number[]>([]);
const selectedNumbersArray = ref<number[][]>([]);
const alertMessage = computed(() =>
	numberArray.value.length ? "" : "已經沒有人名可以產生了!!!"
);
const liFontSize = computed(() => {
	if (randomNumbers.value.length) {
		switch (randomNumbers.value.length) {
			case 1:
				return "10vw";
			case 2:
				return "8vw";
			case 3:
				return "6vw";
			case 4:
				return "6vw";
			case 5:
				return "5.3vw";
		}
	}
	return "4vw";
});

watch(count, (val) => {
	if (val) {
		if (val > 5) {
			count.value = 5;
		} else if (val < 1) {
			count.value = 1;
		}
	}
});

onMounted(() => {
	const numsArray = localStorage.getItem("selectedNumbersArray");
	if (numsArray) {
		let storageCount = 0;
		JSON.parse(numsArray).forEach((numsArr: number[]) => {
			numsArr.forEach((num: number) => {
				storageCount++;
			});
		});
		if (storageCount === endNumber.value - startNumber.value + 1) {
			numberArray.value.length = 0;
		}
		selectedNumbersArray.value = JSON.parse(numsArray);
	}
});

const generateRandomNumbers = () => {
	if (alertMessage.value.length) return;

	randomNumbers.value = [];

	while (
		randomNumbers.value.length < count.value &&
		numberArray.value.length > 0
	) {
		const randomIndex = Math.floor(Math.random() * numberArray.value.length);

		const selectedNumber = numberArray.value.splice(randomIndex, 1)[0];
		if (selectedNumber) {
			randomNumbers.value.push(selectedNumber);
		}
	}
	// 添加到 selectedNumbersArray 中
	selectedNumbersArray.value.push(randomNumbers.value);

	// 更新本地存储
	localStorage.setItem(
		"selectedNumbersArray",
		JSON.stringify(selectedNumbersArray.value)
	);
};

function confirmRestart() {
	const shouldRestart = window.confirm("是否要重新開始？");
	if (shouldRestart) {
		localStorage.removeItem("selectedNumbersArray");
		window.location.reload();
	}
}
</script>

<style scoped lang="scss">
.main-content {
	width: 100%;
	height: 100vh;
	display: flex;
	justify-content: center;
	.img-box {
		position: relative;
		.current-number,
		.circle,
		.btn,
		.inputBox,
		.restart {
			position: absolute;
		}
		.restart {
			bottom: calc(6.3% - 45px);
			left: 50%;
			transform: translateX(-50%);
			z-index: 3;
			// 按鈕樣式
			background-color: #ff6600;
			color: #fff;
			padding: 10px 20px;
			border: none;
			border-radius: 5px;
			cursor: pointer;
			transition: background-color 0.3s ease;

			&:hover {
				background-color: #ff8800;
			}

			&:active {
				background-color: #ff4400;
				transform: translateX(-50%) scale(0.95);
			}
		}
		.inputBox {
			width: 10%;
			height: 100px;
			left: 9%;
			bottom: 83%;
			z-index: 3;
			display: grid;
			label {
				display: none;
			}
			input {
				text-align: center;
				width: 100%;
				border: none;
				outline: none;
				padding: 0;
				margin: 0;
				background: transparent;
				font-size: 2rem;
				color: #000;
			}
			input[type="number"]::-webkit-inner-spin-button,
			input[type="number"]::-webkit-outer-spin-button {
				-webkit-appearance: none;
				appearance: none;
				display: none;
			}
		}
		.btn {
			width: 10%;
			aspect-ratio: 10 / 14;
			top: 0;
			right: 1%;
			z-index: 3;
			cursor: pointer;
			&:active {
				transform: scale(0.95);
			}
			img {
				width: 100%;
				object-fit: cover;
				object-position: center;
			}
		}
		.current-number {
			position: absolute;
			width: 88%;
			height: 70%;
			top: 20%;
			left: 5%;
			z-index: 3;
			display: grid;
			place-items: center;
			p {
				font-size: 4.5vw;
			}
		}
		.circle {
			position: absolute;
			width: 49.8%;
			aspect-ratio: 1;
			top: 3.1%;
			left: 3.3%;
			z-index: 1;
			background-image: linear-gradient(
				90deg,
				#fffbc6 0%,
				#fff22c 50%,
				#fff100 100%
			);
			border-radius: 50%;
			overflow: hidden;

			.selected-number-array {
				width: 50%;
				min-width: 410px;
				margin: 5rem auto;
				height: calc(100% - 10rem);
				display: flex;
				flex-direction: column;
				justify-content: flex-start;
				align-items: center;
				overflow: auto;
				& > li {
					display: flex;
					align-items: center;
					gap: 1rem;
				}
			}
		}
		.main-img {
			position: relative;
			height: 100%;
			object-fit: cover;
			object-position: center;
			z-index: 2;
			pointer-events: none;
		}
	}
}

ul {
	list-style-type: none;
	padding: 0;
}

li {
	margin: 5px 0;
	font-size: 1.5rem;
}

.numberArray {
	width: 100%;
	height: 100%;
	display: flex;
	justify-content: center;
	align-items: center;
	flex-wrap: wrap;
	column-gap: 5vw;
	row-gap: 1vw;

	li {
		--num: 5vw;
		width: calc(var(--num) * 19 / 5);
		text-align: center;
		font-size: var(--num);
		font-weight: 800;
		overflow-wrap: break-word;
		word-wrap: break-word;
	}
}
</style>
