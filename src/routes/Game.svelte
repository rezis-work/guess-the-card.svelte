<script lang="ts">
	import { onMount, createEventDispatcher } from 'svelte';
	import Countdown from './Countdown.svelte';
	import Found from './Found.svelte';
	import Grid from './Grid.svelte';
	import type { Level } from './levels.js';
	import { shuffle } from './utils.js';

	let size: number;
	let grid: string[] = [];
	let founded: string[] = [];
	let remaining: number;
	let duration: number;
	let playing: boolean = false;

	let dispatch = createEventDispatcher();

	export function start(level: Level) {
		size = level.size;
		grid = createGrid(level);
		remaining = duration = level.duration;

		resume();
	}

	function resume() {
		playing = true;
		countDown();

		dispatch('play');
	}

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
				dispatch('won');
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
				dispatch('lost');
				playing = false;
			}
		}

		// onMount
		loop();
	}

	onMount(countDown);
</script>

<div class="game" style="--size: {size}">
	<div class="info">
		{#if playing}
			<Countdown
				{remaining}
				{duration}
				onclick={() => {
					playing = false;
					dispatch('pause');
				}}
			/>
		{/if}
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
