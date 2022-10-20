<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();
	/* let selectedAmount; */
	let selected = 10;

	function getNumber() {
		/* selectedAmount = 'amount=' + selected.toString(); */
		dispatch('existingAmount', {
			data: selected.toString()
		});
	}
</script>

<div class="form-control w-full max-w-xs">
	<label class="label" for="number-input">
	  <span class="label-text">Number of Questions</span>
	  <span class="label-text-alt">Max 50</span>
	</label>
	<input type="number" placeholder="Type here" class="input input-bordered input-primary w-full max-w-xs" bind:value={selected} min="1" max="50" on:change={getNumber} />
 </div>
