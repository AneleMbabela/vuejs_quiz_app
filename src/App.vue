<script setup>
import { ref, computed } from 'vue'
const questions = ref([
	{
    question: 'What is Vue JS?',
    answer: 0,
    options: [
          'A frontend framework',
          'A library',
          'An ice cream maker',      
    ],
    selected: null
  },
  {
    question: 'What is Vuex?',
    answer: 2,
    options: [
          'Vue with an x',
          'A cheese selection',
          'State management library',      
    ],
    selected: null
  },
  {
    question: 'What is Vue Router used for?',
    answer: 1,
    options: [
          'Vue with an x',
          'A routing library for Vue JS',
          'Burger sauce',      
    ],
    selected: null
  },
  {
    question: 'Which of the following statement best define Vue.js?',
    answer: 1,
    options: [
          'Vue.js is an open-source JavaScript library that is used for developing user interfaces.',
          'Vue.js is an open-source front-end JavaScript framework used for developing user interfaces.',
          'Burger sauce',      
    ],
    selected: null
  },
  {
    question: 'Why is Vue.js called a progressive framework?',
    answer: 0,
    options: [
          'Vue.js is called a progressive framework because it is being changed and developed continually.',
          'Vue.js is called a progressive framework because it follows the latest JavaScript standards.',
          'All of the above.',      
    ],
    selected: null
  },
  {
    question: 'What is the main usage of Vue.js?',
    answer: 1,
    options: [
          'Vue.js is a JavaScript library that makes user interfaces for single-page applications by dividing UI into components.',
          'Vue.js is a dynamic JavaScript framework that is frequently used for developing user interfaces.',
          'Vue.js uses the MVVM pattern to bind data to certain DOM elements.',      
    ],
    selected: null
  },
  {
    question: 'Which of the following data binding interpolation is also known as "Mustache" syntax?',
    answer: 0,
    options: [
          '{{}}',
          'v-model',
          'v-on',      
    ],
    selected: null
  },
  {
    question: 'Which of the following is the correct syntax to use for loop in Vue.js?',
    answer: 1,
    options: [
          '*v-for',
          'v-for',
          'vFor',      
    ],
    selected: null
  },
  {
    question: 'How many ways are there to define a filter in Vue.js?',
    answer: 1,
    options: [
          '1',
          '2',
          '3',      
    ],
    selected: null
  },
  {
    question: 'Which of the following method does not represent the non-mutation?',
    answer: 0,
    options: [
          'Reverse() Method',
          'Concat() Method',
          'Slice() Method',      
    ],
    selected: null
  }

])
const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
	let value = 0
	questions.value.map(q => {
		if (q.selected != null && q.answer == q.selected) {
			console.log('correct');
			value++
		}
	})
	return value
})
const getCurrentQuestion = computed(() => {
	let question = questions.value[currentQuestion.value]
	question.index = currentQuestion.value
	return question
})
const SetAnswer = (e) => {
	questions.value[currentQuestion.value].selected = e.target.value
	e.target.value = null
}
const NextQuestion = () => {
	if (currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++
		return
	}
	
	quizCompleted.value = true
}
</script>

<template>
	<main class="app">
		<h1>The Quiz</h1>
		
		<section class="quiz" v-if="!quizCompleted">
			<div class="quiz-info">
				<span class="question">{{ getCurrentQuestion.question }}</span>
				<span class="score">Score {{ score }}/{{ questions.length }}</span>
			</div>
			
			<div class="options">
				<label 
					v-for="(option, index) in getCurrentQuestion.options" 
					:for="'option' + index" 
					:class="`option ${
						getCurrentQuestion.selected == index 
							? index == getCurrentQuestion.answer 
								? 'correct' 
								: 'wrong'
							: ''
					} ${
						getCurrentQuestion.selected != null &&
						index != getCurrentQuestion.selected
							? 'disabled'
							: ''
					}`">
					<input 
						type="radio" 
						:id="'option' + index" 
						:name="getCurrentQuestion.index" 
						:value="index" 
						v-model="getCurrentQuestion.selected" 
						:disabled="getCurrentQuestion.selected"
						@change="SetAnswer" 
					/>
					<span>{{ option }}</span>
				</label>
			</div>
			
			<button 
				@click="NextQuestion" 
				:disabled="!getCurrentQuestion.selected">
				{{ 
					getCurrentQuestion.index == questions.length - 1 
						? 'Finish' 
						: getCurrentQuestion.selected == null
							? 'Select an option'
							: 'Next question'
				}}
			</button>
		</section>

		<section v-else>
			<h2>You have finished the quiz!</h2>
			<p>Your score is {{ score }}/{{ questions.length }}</p>
		</section>
	</main>
</template>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'Montserrat', sans-serif;
}
body {
	background-color: #222b35;
	color: #FFF;
}
.app {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 2rem;
	height: 100vh;
}
h1 {
	font-size: 2rem;
	margin-bottom: 2rem;
}
.quiz {
	padding: 1rem;
	width: 100%;
	max-width: 640px;
}
.quiz-info {
	display: flex;
	justify-content: space-between;
	margin-bottom: 1rem;
}
.quiz-info .question {
	color: #8F8F8F;
	font-size: 1.25rem;
}
.quiz-info.score {
	color: #FFF;
	font-size: 1.25rem;
}
.options {
	margin-bottom: 1rem;
}
.option {
	padding: 1rem;
	display: block;
	background-color: #86818f;
	margin-bottom: 0.5rem;
	border-radius: 0.5rem;
	cursor: pointer;
}
.option:hover {
	background-color: #a09aa8;
}
.option.correct {
	background-color: #2cce7d;
}
.option.wrong {
	background-color: #ff5a5f;
}
.option:last-of-type {
	margin-bottom: 0;
}
.option.disabled {
	opacity: 0.5;
}
.option input {
	display: none;
}
button {
	appearance: none;
	outline: none;
	border: none;
	cursor: pointer;
	padding: 0.5rem 1rem;
	background-color: #d6eee2;
	color: #4b4b4d;
	font-weight: 700;
	text-transform: uppercase;
	font-size: 1.2rem;
	border-radius: 0.5rem;
}
button:disabled {
	opacity: 0.5;
}
h2 {
	font-size: 2rem;
	margin-bottom: 2rem;
	text-align: center;
}
p {
	color: #8F8F8F;
	font-size: 1.5rem;
	text-align: center;
}
</style>