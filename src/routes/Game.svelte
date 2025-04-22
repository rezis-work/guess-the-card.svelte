<script lang="ts">
	import { onMount } from 'svelte';
	import Countdown from './Countdown.svelte';
	import Found from './Found.svelte';
	import Grid from './Grid.svelte';
	import { levels } from './levels.js';
	import type { Level } from './levels.js';
	import { shuffle } from './utils.js';

	const level = levels[0];

	let size: number = level.size;
	let grid: string[] = createGrid(level);
	let founded: string[] = [];
	let remaining: number = level.duration;
	let duration: number = level.duration;
	let playing: boolean = true;

	function createGrid(level: Level): string[] {
		const copy = level.emojis.slice();
		const pairs: string[] = [];

		for (let i = 0; i < (level.size * level.size) / 2; i += 1) {
			const index = Math.floor(Math.random() * copy.length);
			const emojis = copy[index];

			copy.splice(index, 1);
			pairs.push(emojis);
		}

		pairs.push(...pairs);

		return shuffle(pairs);
	}

	function found(event: string, data: { emoji: string }) {
		if (event === 'found') {
			founded = [...founded, data.emoji];

			if (founded.length === (size * size) / 2) {
				playing = false;
			}
		}
	}

	function countDown() {
		const start = Date.now();
		let remaining_at_start = remaining;

		function loop() {
			if (!playing) return;
			requestAnimationFrame(loop);

			remaining = remaining_at_start - (Date.now() - start);

			if (remaining <= 0) {
				// TODO: Game over
				playing = false;
			}
		}

		// onMount
		loop();
	}

	onMount(countDown);
</script>

<div class="game">
	<div class="info">
		<Countdown
			{remaining}
			{duration}
			onclick={() => {
				// TODO:s
			}}
		/>
	</div>
	<div class="grid-container">
		<Grid {grid} {found} {founded} />
	</div>
	<div class="info">
		<Found {founded} />
	</div>
</div>

<style>
	.game {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
		font-size: min(1vmin, 0.5rem);
	}

	.info {
		width: 80em;
		height: 10em;
	}

	.grid-container {
		width: 80em;
		height: 80em;
	}
</style>
