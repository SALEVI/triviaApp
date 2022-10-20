<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();
	const difficulties = [
		{ id: 'any', text: `Any Difficulty` },
		{ id: 'easy', text: `Easy` },
		{ id: 'medium', text: `Medium` },
		{ id: 'hard', text: `Hard` }
	];

	let selected;

	function getDifficulty() {
		if (selected.id == 'any') {
			dispatch('existingDifficulty', {
				data: ''
			});
		} else {
			dispatch('existingDifficulty', {
				data: '&difficulty=' + selected.id
			});
		}
	}
</script>

<div class="form-control w-full max-w-xs">
		<label class="label">
		  <span class="label-text">Select Difficulty</span>
		</label>
		<select class="select select-primary w-full max-w-xs" bind:value={selected} on:change={getDifficulty}>
			{#each difficulties as difficulty}
				<option value={difficulty}>
					{difficulty.text}
				</option>
			{/each}
		</select>
</div>

