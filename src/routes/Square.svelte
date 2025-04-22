<script lang="ts">
	import { send } from './transitions.js';
	let {
		emoji,
		selected,
		onclick,
		founded,
		group
	}: { emoji: string; selected: boolean; onclick: () => void; founded: boolean; group: 'a' | 'b' } =
		$props();
</script>

<div class="square" class:flipped={selected || founded}>
	<button {onclick} aria-label={emoji} aria-hidden={!selected}></button>

	<div class="background"></div>

	{#if !founded}
		<span out:send={{ key: `${emoji}${group}` }}>{emoji}</span>
	{/if}
</div>

<style>
	.square {
		display: flex;
		justify-content: center;
		align-items: center;
		transform-style: preserve-3d;
		transition: transform 0.4s;
	}

	button {
		position: absolute;
		width: 100%;
		height: 100%;
		backface-visibility: hidden;
		background-color: #eee;
		border: none;
		border-radius: 1em;
		font-size: inherit;
	}

	.background {
		background-color: white;
		border: 0.2em solid #ccc;
		border-radius: 1em;
		position: absolute;
		transform: rotateY(180deg);
		backface-visibility: hidden;
		width: 100%;
		height: 100%;
	}

	.flipped {
		transform: rotateY(180deg);
	}

	span {
		font-size: 5em;
		transform: rotateY(180deg);
		pointer-events: none;
		backface-visibility: hidden;
	}
</style>
