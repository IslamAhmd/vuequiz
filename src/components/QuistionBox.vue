<template>

	<div class="quistion-box-container">
	  <b-jumbotron>

		    <template v-slot:lead>
		      {{currentQuestion.question}}
		    </template>

		    <hr class="my-4">


		    <b-list-group>
			  <b-list-group-item
			    v-for="(answer, index) in answers"
			    :key="index"
			    @click="selectAnswer(index)"
			    :class="[selectedIndex === index ? 'selected':'']" >

			  		{{answer}}

			  </b-list-group-item>
			</b-list-group>


		    <b-button variant="primary" 
		    		@click="submitAnswer">Submit</b-button>
		    <b-button variant="success" href="#" @click="next">Next</b-button>

    	</b-jumbotron>
	</div>

</template>

<script>
	
	import _ from 'lodash'

	export default {
		props: {
			currentQuestion: Object,
			next: Function,
			increment: Function
		},
		data() {
			return {
				selectedIndex: null,
				correctIndex: null,
				shuffledAnswers: []
			}
		},
		computed: {
			answers(){
				let answers = this.currentQuestion.incorrect_answers
				answers.push(this.currentQuestion.correct_answer)
				return answers
			}
		},
		watch: {
			currentQuestion: {
				immediate: true,
				handler() {
					this.selectedIndex = null
					this.shuffleAnswers()
				}
			}
		},
		methods: {
			selectAnswer(index) {
				this.selectedIndex = index
			},
			submitAnswer(){
				let isCorrect = false
				if(this.selectedIndex === this.correctIndex){
					isCorrect = true
				}
				this.increment(isCorrect)
			},
			shuffleAnswers() {
				let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
				this.shuffledAnswers = _.shuffle(answers)
			}
		},
	}


</script>

<style> 

	.list-group {
		margin-bottom: 15px;
	}

	.list-group-item:hover {
		background: #EEE;
		cursor: pointer;
	}

	.selected {
		background: lightblue;
	}

	.correct {
		background: lightgreen;
	}

	.incorrect {
		background: lightred;
	}

</style>