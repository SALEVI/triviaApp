<script>
	import { fade, fly } from 'svelte/transition';

	export let question;
	export let nextQuestion;
	export let addToScore;
	export let score;
	export let questionNumber;
	export let numberOfQuestions;

	$: clientWidth = document.body.clientWidth;

	let isCorrect;
	let isAnswered = false;

	let answers = question.incorrect_answers.map((answer) => {
		return {
			answer,
			correct: false
		};
	});

	let allAnswers = [
		...answers,
		{
			answer: question.correct_answer,
			correct: true
		}
	];

	shuffle(allAnswers);

	function shuffle(array) {
		array.sort(() => Math.random() - 0.5);
	}

	function checkQuestion(correct) {
		isAnswered = true;
		isCorrect = correct;
		if (correct) {
			addToScore();
		}
	}

/* 	function setGreen(answered, correct, index){
		if (answered){
			let selectedBtn = document.querySelectorAll('.btnSelector');
			for (let elem of selectedBtn){
				elem.classList.add("btn-error");
			}
			console.log(index);
			if (correct) {
				let selected = document.getElementById(index);
				selected.classList.remove("btn-error");
				selected.classList.add("btn-success");
			}
		}
	} */
	//goes inside button
	/*on:click|once={setGreen(isAnswered, answer.correct, index)}*/
	
</script>

<section class="wrapper">

	<div class="stat">
      <div class="stat-title forQuestion">Question</div>
      <div class="stat-value text-secondary">{questionNumber}</div>
   </div>
   <div class="stat score"> 
      <div class="stat-title forScore">Score</div>
      <div class="stat-value text-primary">{score}</div>
   </div>
	<div class="moveme">
		<progress class="progress progress-secondary w-56" value={questionNumber} max={numberOfQuestions}></progress>
	</div>

	<h1 class="font-sans 2xl:text-3xl text-2xl antialiased font-semibold text-info">{@html question.question}</h1>
	<div class="badges">
		<div class="badge badge-primary badge-outline">{question.category}</div>
		<div class="badge badge-secondary badge-outline">{question.difficulty}</div>
		<div class="badge badge-accent badge-outline">
			{#if question.type == 'boolean'}
				true or false
			{:else}
				multiple
			{/if}
		</div>
	</div>
{#if isAnswered}
<div class="answer">
	{#if isCorrect}
	<div class="alert alert-success shadow-lg lg:text-lg sm:text-sm Ans" in:fly={{x:-100}}>
		<div class="fix">
		  <svg xmlns="http://www.w3.org/2000/svg" class="stroke-current flex-shrink-0 h-6 w-6" fill="none" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
		  <span>Correct!</span>
		</div>
	 </div>
	{:else}
	<div class="alert alert-error shadow-lg lg:text-lg sm:text-sm Ans" in:fly={{x:-100}}>
		<div class="fix">
		  <svg xmlns="http://www.w3.org/2000/svg" class="stroke-current flex-shrink-0 h-6 w-6" fill="none" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
		  <span>Incorrect</span>
		</div>
	 </div>
	<div class="alert alert-success shadow-lg lg:text-lg sm:text-sm correctAns" in:fly={{x:100}}>
		<div class="fix">
		  <svg xmlns="http://www.w3.org/2000/svg" class="stroke-current flex-shrink-0 h-6 w-6" fill="none" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
		  <span>
			{#if clientWidth >= 555}
			Correct answer: {@html question.correct_answer}
			{:else}
			{@html question.correct_answer}
			{/if}
			</span>
		</div>
	 </div>
	{/if}
</div>
{/if}

<div class="grid grid-cols-2 gap-8 text-center justify-evenly div-answers">
	{#each allAnswers as answer, index}
		<button id={index} class="btn lg:btn-lg sm:btn-sm box btnSelector btn-answers" on:click={isAnswered? '' : () => checkQuestion(answer.correct)}> 
			{@html answer.answer}
	</button>
	{/each}
</div>

{#if isAnswered}
	<div class="space">
		<button class="btn btn-active btn-primary btn-block" in:fly={{y:30}} on:click={nextQuestion}>Next Question</button>
	</div>
{/if}

</section>
<style>
/* 	.btn-answers {
		width: fit-content;
	} */
	.fix {
		margin-top:auto;
		margin-bottom: auto;
		margin-right: auto;
	}
	.answer {
		display: flex;
		width: auto;
		padding-bottom: 2em;
		gap: 2rem;
	}
	.Ans {
		float: left;
	}
	.correctAns {
		float: right;
	}
	section {
		max-width: 90vw;
		max-height: 100vh;
		overflow: auto;
	}
	progress {
		width: 100%;
	}
	.space {
		padding-top: 2em;
		display: flex;
		align-self: center;
		justify-content: center;
	}
	h1 {
		padding-top: .5em;
	}
	.badges {
		padding-top: .5em;
		padding-bottom: 2em;
	}
	.wrapper {
		padding-left: 2em;
		padding-right: 2em;
      padding: 3em;
		overflow: auto;
		/* box-shadow: rgba(17, 17, 26, 0.1) 0px 0px 16px; */
		/* backdrop-filter: blur(5px); */
	}

	.stat {
		padding: 0;
		width: auto;
	}
	.score {
		text-align: right;
		float: right;
	}

	@media only screen and (max-width: 465px){
   .wrapper {
		padding: 0;

	}
	.forQuestion {
		color: hsl(var(--s));
	}
	.forScore {
		color: hsl(var(--p));
	}
	.btn-answers {
  		white-space: wrap;
  		overflow:hidden;
  		text-overflow:ellipsis;
	}
	.div-answers {
		gap: .5em;
	}

	.answer {
		gap: .5em;
	}
} 
</style>