<script setup>
import { ref, computed, onMounted } from 'vue'
import tips from "./helpers/tips.js";

const wins = ref(0)
const draws = ref(0)
const losses = ref(0)

const choice = ref(null)
const computerChoice = ref(null)
const verdict = ref(null)

const tipChoice = ref(null)
const tipsChoices = tips

const outcomes = {
	'plastic': {
		'plastic': 'draw',
		'people': 'loss',
		'tierra': 'win'
	},
	'people': {
		'plastic': 'win',
		'people': 'draw',
		'tierra': 'loss'
	},
	'earth': {
		'plastic': 'loss',
		'people': 'win',
		'earth': 'draw'
	}
}

const play = (c) => {
	choice.value = c
	
	const choices = ['plastic', 'people', 'earth']
	const random = Math.floor(Math.random() * choices.length)
	computerChoice.value = choices[random]

	const randomTips = Math.floor(Math.random() * tipsChoices.length)
	tipChoice.value = tipsChoices[randomTips]

	const outcome = outcomes[choice.value][computerChoice.value]
	
	if (outcome === 'win') {
		wins.value++
		verdict.value = 'You win!'
	} else if (outcome === 'loss') {
		losses.value++
		verdict.value = 'You Lose!'
	} else {
		draws.value++
		verdict.value = 'It\'s a draw'
	}

	SaveGame()
}

const winPercentage = computed(() => {
	const total = wins.value + draws.value + losses.value
	return total ? (wins.value / total) * 100 : 0
})

const SaveGame = () => {
	localStorage.setItem('wins', wins.value)
	localStorage.setItem('draws', draws.value)
	localStorage.setItem('losses', losses.value)
	localStorage.setItem('tips', tipChoice.value)
}

const LoadGame = () => {
	wins.value = parseInt(localStorage.getItem('wins')) || 0
	draws.value = parseInt(localStorage.getItem('draws')) || 0
	losses.value = parseInt(localStorage.getItem('losses')) || 0
	tipChoice.value = parseInt(localStorage.getItem('tips')) || ''
}

const ResetRound = () => {
	choice.value = null
	computerChoice.value = null
	verdict.value = null
}

onMounted(() => {
	LoadGame()

	window.addEventListener('keypress', (e) => {
		if (e.key === 'r') {
			ResetRound()
		}
	})
})
</script>

<template>
	<div class="bg-gray-700 text-white text-center min-h-screen flex flex-col">
		<header class="container mx-auto p-6">
			<h1 class="text-4xl font-bold">Plastic, People, Earth!</h1>
		</header>

		<main class="container mx-auto p-6 flex-1">
			<div v-if="choice === null" class="flex items-center justify-center -mx-6">

				<button @click="play('plastic')"
					class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-blue-500">
					<img src="./assets/plastic.svg" alt="Rock" class="w-full" />
				</button>

				<button @click="play('people')"
					class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-yellow-500">
					<img src="./assets/employees.svg" alt="Paper" />
				</button>

				<button @click="play('earth')"
					class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-green-500">
					<img src="./assets/worldwide.svg" alt="Scissors" />
				</button>

			</div>

			<div v-else>
				<div class="text-3xl mb-4">
					You picked <span class="text-pink-500">{{ choice }}</span>
				</div>
				<div class="text-3xl mb-4">
					The computer picked <span class="text-green-500">{{ computerChoice }}</span>
				</div>
				<div class="text-6xl mb-12">
					{{ verdict }}
				</div>

				<button @click="ResetRound" class="bg-pink-500 text-lg py-2 px-4">Reset</button>
			</div>

			<div class="mt-12 text-3xl mb-4">{{ wins }} : {{ draws }} : {{ losses }}</div>

			<div class="text-lg">Win rate: {{ Math.round(winPercentage) }}%</div>
			<br>
			<div class="text-lg"><b>{{ tipChoice }}</b></div>
		</main>

		<footer class="container mx-auto p-6">
			<a href="https://www.vecteezy.com/free-vector/rock-paper-scissors-game">Rock Paper Scissors Game Vectors by
				Vecteezy</a>

				<p> Diana Cuenca Adaptation</p>
				<p>Recycling tips from:<a href="https://www.recyclingbins.co.uk/recycling-tips/"> https://www.recyclingbins.co.uk/recycling-tips/</a> </p>
		</footer>
	</div>
</template>

<style></style>
