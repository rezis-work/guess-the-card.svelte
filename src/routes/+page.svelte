<script lang="ts">
	import { levels } from './levels.js';
	import Modal from './Modal.svelte';
	import Game from './Game.svelte';
	import { confetti } from '@neoconfetti/svelte';
	import '../styles.css';

	let state: 'waiting' | 'playing' | 'paused' | 'won' | 'lost' = $state('waiting');
	let game: Game;
</script>

<Game
	bind:this={game}
	on:play={() => {
		state = 'playing';
	}}
	on:pause={() => {
		state = 'paused';
	}}
	on:resume={() => {
		state = 'playing';
	}}
	on:won={() => {
		state = 'won';
	}}
	on:lost={() => {
		state = 'lost';
	}}
/>

{#if state !== 'playing'}
	<Modal>
		<header>
			<h1><span>line</span> Match</h1>
			<p>the emoji matching game</p>
		</header>
		{#if state === 'won' || state === 'lost'}
			<p>you {state} the game!</p>
		{:else if state === 'paused'}
			<p>game paused</p>
		{:else if state === 'waiting'}
			<p>choose level:</p>
		{/if}

		<div class="buttons">
			{#if state === 'paused'}
				<button onclick={() => game.resume()}>resume</button>
			{:else}
				{#each levels as level}
					<button onclick={() => game.start(level)}>{level.label}</button>
				{/each}
			{/if}
		</div>
	</Modal>
{/if}

{#if state === 'won'}
	<div
		class="confetti"
		use:confetti={{
			stageWidth: innerWidth,
			stageHeight: innerHeight
		}}
	></div>
{/if}

<style>
	h1 {
		font-size: 4em;
	}
	h1 span {
		color: purple;
	}

	p {
		font-family: 'Grandstander', cursive;
	}

	.confetti {
		position: fixed;
		width: 100%;
		height: 100%;
		left: 50%;
		top: 50%;
		pointer-events: none;
	}
</style>
