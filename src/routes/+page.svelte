<script>
   import badGif from '$lib/images/bad.gif';
   import excelentGif from '$lib/images/excelent.gif';
   import fairGif from '$lib/images/fair.gif';
   import goodGif from '$lib/images/good.gif';
   import OpendbLogo from '$lib/images/opendblogo.png';
   import poorGif from '$lib/images/poor.gif';
   import loadingGif from '$lib/images/puff.svg';
   import Options from '$lib/Options/Options.svelte';
   import Question from '$lib/Question.svelte';
   import { fade, fly } from 'svelte/transition';

	const apiDefURL = 'https://opentdb.com/api.php?';
   let apiData;
	let apiURL;
	let hideOptions = false;
	let score = 0;
	let activeQuestion = 0;
	let quiz = getQuiz();
	let active = false;
   let numberOfQuestions;
	$: hideStart = true;
	$: showQuiz = false;

	function handleCreator(event) {
		apiData = event.detail.data;
      numberOfQuestions = parseInt((apiData.amount).substring(7));
		console.log(apiURL);
	}

	function handleOptions(event) {
		hideOptions = event.detail.data;
		hideStart = false;
	}

	async function getQuiz() {
		const res = await fetch(
			`https://opentdb.com/api.php?${apiData.amount}${apiData.category}${apiData.difficulty}${apiData.type}`
		);
		const quiz = await res.json();
		return quiz;
	}

	function startQuiz() {
		score = 0;
		activeQuestion = 0;
		quiz = getQuiz();
	}

	function addToScore() {
		score = score + 1;
	}

	function nextQuestion() {
		activeQuestion = activeQuestion + 1;
	}

   $: questionNumber = activeQuestion + 1;

   $: scorePercentage = Math.floor((score * 100)/numberOfQuestions);

</script>

<section>
<div class="wrapper">
	{#if !hideOptions}
      <h1>TRIVIA</h1>
      <div class="options">
		   <Options on:createdData={handleCreator} on:hideOptions={handleOptions} />
      </div>
	{/if}

	{#if !hideStart}
   <div class="box-2">
		<button
         class="buton buton-two font-sans font-semibold text-lg"
			class:notActive={active}
			on:click|once={startQuiz}
			on:click={(showQuiz = true)}
			on:click={() => (active = !active)}>START</button
		>
   </div>
	{/if}

	{#if (showQuiz = true)}
		{#await quiz}
         {#if !hideStart}
			<img class="loadingImg" src={loadingGif} alt="loading gif"/>
         {/if}
		{:then data}
			{#each data.results as question, index}
				{#if index === activeQuestion}
					<div in:fly={{x:100}} out:fly={{x:-100}} class="fade-wrapper">
						<Question {addToScore} {nextQuestion} {question} {score} {questionNumber} {numberOfQuestions}/>
					</div>
				{/if}
			{/each}
		{/await}
	{/if}

   {#if activeQuestion == numberOfQuestions}
      
      <div class="endScreen" transition:fade >

         <div class="endImg">
            {#if (scorePercentage>= 0) && (scorePercentage <20)} 
            <img src={badGif} alt="Simpsons all judges rate zero" />
            {/if}
            {#if (scorePercentage>= 20) && (scorePercentage <40)} 
            <img src={poorGif} alt="Dark Souls get good meme" />
            {/if}
            {#if (scorePercentage>= 40) && (scorePercentage <60)} 
            <img src={fairGif} alt="Man saying it's not that bad" />
            {/if}
            {#if (scorePercentage>= 60) && (scorePercentage <80)} 
            <img src={goodGif} alt="Leonardo DiCaprio clapping" />
            {/if}
            {#if (scorePercentage>= 80) && (scorePercentage <=100)} 
            <img src={excelentGif} alt="All judges rate ten" />
            {/if}
         </div>
         <div class="stats shadow">
  
            <div class="stat sm:w-full w-3/4 h-3/4 sm:h-full">
              <div class="stat-figure text-primary">
               <div class="radial-progress text-primary" style="--value:{scorePercentage};">{scorePercentage}</div>
              </div>
              <div class="stat-title">Total Score</div>
              <div class="stat-value text-primary">{score}</div>
              <div class="stat-desc">
               {#if (scorePercentage>= 0) && (scorePercentage <20)}   
               Bad
               {/if}
               {#if (scorePercentage>= 20) && (scorePercentage <40)} 
               Poor
               {/if}
               {#if (scorePercentage>= 40) && (scorePercentage <60)} 
               Fair
               {/if}
               {#if (scorePercentage>= 60) && (scorePercentage <80)} 
               Good
               {/if}
               {#if (scorePercentage>= 80) && (scorePercentage <=100)} 
               Excelent
               {/if}
              </div>
            </div>     
            
            <div class="stat endButtons">
                  <button class="btn btn-active btn-primary btn-sm" on:click={() => window.location.reload()}>Options</button>
                  <button class="btn btn-active btn-primary btn-sm" on:click={ startQuiz}>Restart</button>
            </div>  
          </div>
          <div class="font-title mb-2 font-medium text-lg footerText">Made with</div>
         <span class="footer flex flex-row justify-center items-center">
         <img class="w-28 h-28 opendtdb" src={OpendbLogo} alt="opendtdb logo" />
         <span class="font-title mb-2 text-4xl font-extrabold sm:text-3xl lg:text-4xl">daisyUI</span>
      </span>
      </div>
   {/if}
</div>
</section>

<style>
   .loadingImg {
      position: absolute;
   }
   .footerText {
      margin-top: 2em;
      margin-bottom: .5em;
      margin-left: auto;
      margin-right: auto;
   }
   .footer {
      gap: 2em;
   }
   section {
      background-image: url("../lib/images/bg.png");
      background-size: cover;
   } 
   .options {
      backdrop-filter: blur(10px);
      padding: 2em;
      background-color: rgba(0, 0, 0, 0.1);
   }
   .endButtons {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
   }
   .stats {
      margin-left: auto;
      margin-right: auto;
   }
   .endScreen {
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      align-items: flex-start;
      box-shadow: rgba(0, 0, 0, 0.16) 0px 3px 6px, rgba(0, 0, 0, 0.23) 0px 3px 6px;
      border: 1px solid #6419E6;
      border-radius: 8px;
      padding: 3em;
      background: rgba(0, 0, 0, 0.4);
      backdrop-filter: blur(5px);
   }
   .endImg {
      max-width: 500px;
      max-height: 500px;
      margin-left: auto;
      margin-right: auto;
      object-fit: cover;
      padding-bottom: 2em;
   }

   img {
      border-radius: 8px;
      max-width: 100%;
      height: auto;
   }
   .fade-wrapper{
      position:absolute;
   }
	.notActive {
		display: none;
	}
   .wrapper {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      max-width: 100vw;
      max-height: 100vh;
      padding: 50vh 1.5em;
      overflow: hidden;
      /* backdrop-filter: blur(5px); */
   }

   @font-palette-values --Custom {
      font-family: Nabla;
      base-palette: 4;
   }
   h1 {
      font-family:'Nabla';
      font-size: 10rem;
      line-height: .5em;
      margin-bottom: .6em;
      font-palette: --Custom;
      -webkit-user-select: none;  /* Chrome all / Safari all */
      -moz-user-select: none;     /* Firefox all */
      -ms-user-select: none;      /* IE 10+ */
       user-select: none;  
      margin-top: .5em;
   }
   div[class*=box] {
  height: 33.33%;
  width: 100%; 
  display: flex;
  justify-content: center;
  align-items: center;
}

.box-2 { background-color:  ''}


.buton {
  line-height: 50px;
  height: 50px;
  text-align: center;
  width: 250px;
  cursor: pointer;
}
.buton-two {
  color: #FFF;
  transition: all 0.5s;
  position: relative; 
}
.buton-two::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  transition: all 0.5s;
  border: 1px solid rgba(255,255,255,0.2);
  background-color: rgba(255,255,255,0.1);
}
.buton-two::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  transition: all 0.5s;
  border: 1px solid rgba(255,255,255,0.2);
  background-color: rgba(255,255,255,0.1);
}
.buton-two:hover::before {
  transform: rotate(-45deg);
  background-color: rgba(255,255,255,0);
}
.buton-two:hover::after {
  transform: rotate(45deg);
  background-color: rgba(255,255,255,0);
}

@media only screen and (max-width: 465px){
   .endScreen {
     border: none;
     background: transparent;
     box-shadow: none;
   }

   .fade-wrapper{
      left: auto;
      right: auto;
      top: 8%;
      bottom: auto;
      width: auto;
      height: auto;
   }

   .wrapper {
      overflow: auto;
   }
}

@media only screen and (max-width: 765px) {
   h1 {
      font-size: 8vh;
   }
}
</style>
