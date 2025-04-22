<script lang="ts">
	import Square from './Square.svelte';

	let {
		grid,
		found,
		founded
	}: {
		grid: string[];
		found: (event: string, data: { emoji: string }) => void;
		founded: string[];
	} = $props();

	let a: number = $state(-1);
	let b: number = $state(-1);
	let resetTimeout: number;
</script>

<div class="grid">
	{#each grid as emoji, i}
		<Square
			{emoji}
			onclick={() => {
				clearTimeout(resetTimeout);
				if (a === -1 && b === -1) {
					a = i;
				} else if (b === -1) {
					b = i;
					if (grid[a] === grid[b]) {
						// correct
						found('found', {
							emoji
						});
						console.log('correct');
					} else {
						// incorect
						resetTimeout = setTimeout(() => {
							a = b = -1;
						}, 1000);
					}
				} else {
					b = -1;
					a = i;
				}
			}}
			selected={a === i || b === i}
			founded={founded.includes(emoji)}
			group={grid.indexOf(emoji) === i ? 'a' : 'b'}
		/>
	{/each}
</div>

<style>
	.grid {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		grid-template-rows: repeat(4, 1fr);
		grid-gap: 0.5em;
		height: 100%;
		perspective: 1000vw;
	}
</style>
