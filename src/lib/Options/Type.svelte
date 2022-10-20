<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();
	const types = [
		{ id: 'any', text: `Any Type` },
		{ id: 'multiple', text: `Multiple Choice` },
		{ id: 'boolean', text: `True or False` }
	];

	let selected;

	function getType() {
		if (selected.id == 'any') {
			dispatch('existingType', {
				data: ''
			});
		} else {
			dispatch('existingType', {
				data: '&type=' + selected.id
			});
		}
	}
</script>

<div class="form-control w-full max-w-xs">
	<label class="label" for="type-input">
	  <span class="label-text">Select Type</span>
	</label>
	<select class="select select-primary w-full max-w-xs" bind:value={selected} on:change={getType}>
		{#each types as type}
			<option value={type}>
				{type.text}
			</option>
		{/each}
	</select>
</div>