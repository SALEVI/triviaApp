<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();
	const categories = [
		{ id: 'any', text: `Any Category` },
		{ id: '9', text: `General Knowledge` },
		{ id: '10', text: `Entertainment: Books` },
		{ id: '11', text: `Entertainment: Film` },
		{ id: '12', text: `Entertainment: Music` },
		{ id: '13', text: `Entertainment: Musicals & Theatres` },
		{ id: '14', text: `Entertainment: Television` },
		{ id: '15', text: `Entertainment: Video Games` },
		{ id: '16', text: `Entertainment: Board Games` },
		{ id: '17', text: `Science & Nature` },
		{ id: '18', text: `Science: Computers` },
		{ id: '19', text: `Science: Mathematics` },
		{ id: '20', text: `Mythology` },
		{ id: '21', text: `Sports` },
		{ id: '22', text: `Geography` },
		{ id: '23', text: `History` },
		{ id: '24', text: `Politics` },
		{ id: '25', text: `Art` },
		{ id: '26', text: `Celebrities` },
		{ id: '27', text: `Animals` },
		{ id: '28', text: `Vehicles` },
		{ id: '29', text: `Entertainment: Comics` },
		{ id: '30', text: `Science: Gadgets` },
		{ id: '31', text: `Entertainment: Japanese Anime & Manga` },
		{ id: '32', text: `Entertainment: Cartoon & Animations` }
	];

	let selected;

	function getCategory() {
		if (selected.id == 'any') {
			dispatch('existingCategory', {
				data: ''
			});
		} else {
			dispatch('existingCategory', {
				data: '&category=' + selected.id
			});
		}
	}
</script>

<div class="form-control w-full max-w-xs">
		<label class="label">
		  <span class="label-text">Select Category</span>
		</label>
		<select
		class="select select-primary w-full max-w-xs"
		bind:value={selected}
		on:change={getCategory}
	>
		{#each categories as category}
			<option value={category}>
				{category.text}
			</option>
		{/each}
	</select>
	</div>

