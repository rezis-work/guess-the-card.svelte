<script lang="ts">
	import { levels } from './levels';
	import Modal from './Modal.svelte';
	import Game from './Game.svelte';
	import '../styles.css';

	let state: 'waiting' | 'playing' | 'paused' | 'won' | 'lost' = $state('waiting');
</script>

<Game />

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
				<button>resume</button>
			{:else}
				{#each levels as level}
					<button>{level.label}</button>
				{/each}
			{/if}
		</div>
	</Modal>
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
</style>
