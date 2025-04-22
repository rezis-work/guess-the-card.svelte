<script lang="ts">
	import { receive } from './transitions.js';
	let { founded }: { founded: string[] } = $props();
</script>

<div class="found">
	{#each founded as emoji (emoji)}
		<div class="pair">
			<span in:receive={{ key: `${emoji}a` }}>{emoji}</span>
			<span in:receive={{ key: `${emoji}b` }}>{emoji}</span>
		</div>
	{/each}
</div>

<style>
	.found {
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 0.5em;
		height: 100%;
		filter: drop-shadow(0 0 0.5em #000);
	}

	.pair {
		width: 4em;
		height: 4em;
		background-color: #fff;
		border-radius: 1em;
		border-radius: 50%;
	}
	span {
		position: absolute;
		font-size: 2.6em;
	}
</style>
