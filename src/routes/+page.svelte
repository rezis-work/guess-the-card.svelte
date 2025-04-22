<script lang="ts">
	import { levels } from './levels.js';
	import Modal from './Modal.svelte';
	import Game from './Game.svelte';
	import { confetti } from '@neoconfetti/svelte';
	import '../styles.css';

	let state: 'waiting' | 'playing' | 'paused' | 'won' | 'lost' = $state('waiting');
	let game: Game;
</script>

<svelte:head>
	<title>line match</title>
	<link rel="icon" href="/linelogo.png" />
	<meta name="description" content="the emoji matching game" />
</svelte:head>

<main>
	<Game
		bind:this={game}
		on:play={() => {
			state = 'playing';
		}}
		on:pause={() => {
			state = 'paused';
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
					<button onclick={() => (state = 'waiting')}>quit</button>
				{:else}
					{#each levels as level}
						<button onclick={() => game.start(level)}>{level.label}</button>
					{/each}
				{/if}
			</div>
		</Modal>
	{/if}
</main>

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
	main {
		text-align: center;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
	}

	header {
		font-size: min(5vw, 2rem);
		font-family: Grandstander;
	}

	h1 {
		font-size: 4em;
		margin: 0;
		height: 1em;
	}

	h1 span {
		color: var(--accent);
	}

	p {
		margin: 0 0 1em 0;
	}

	.buttons {
		display: flex;
		justify-content: center;
		gap: 0.5em;
	}

	button {
		background: var(--accent);
		color: white;
		font-size: inherit;
		font-family: inherit;
		border: none;
		padding: 1em;
		border-radius: 0.5em;
		cursor: pointer;
		transition: background 0.2s ease-in-out;
	}

	button:hover {
		background: var(--accent-dark);
	}

	.confetti {
		position: fixed;
		width: 100%;
		height: 100%;
		left: 50%;
		top: 30%;
		pointer-events: none;
	}
</style>
