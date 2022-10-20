<script>
	import { createEventDispatcher } from 'svelte';
	import AmountQuestions from './AmountQuestions.svelte';
	import Category from './Category.svelte';
	import Difficulty from './Difficulty.svelte';
	import Type from './Type.svelte';

	const dispatch = createEventDispatcher();

	let apiURL = '';
	let apiData = {
		amount: 'amount=10',
		category: '',
		difficulty: '',
		type: ''
	};

	// api URL generator functions
	function handleAmount(event) {
		apiData.amount = 'amount=' + event.detail.data;
		console.log(apiData.amount);
	}
	function handleCategory(event) {
		apiData.category = event.detail.data;
		console.log(apiData.category);
	}
	function handleDifficulty(event) {
		apiData.difficulty = event.detail.data;
		console.log(apiData.difficulty);
	}
	function handleType(event) {
		apiData.type = event.detail.data;
		console.log(apiData.type);
	}

	function getApiURL() {
		dispatch('createdData', {
			data: apiData
		}),
			dispatch('hideOptions', {
				data: true
			});
	}
</script>

<section>
	<AmountQuestions on:existingAmount={handleAmount} />
	<Category on:existingCategory={handleCategory} />
	<Difficulty on:existingDifficulty={handleDifficulty} />
	<Type on:existingType={handleType} />
<button class="btn btn-block btn-active btn-primary" on:click={getApiURL}>SAVE</button>
</section>
<style>
	button {
		margin-top: 3em;
	}
</style>