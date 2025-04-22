<script lang="ts">
	import Modal from './Modal.svelte';
	import Game from './Game.svelte';
	import '../styles.css';

	let state: 'waiting' | 'playing' | 'paused' | 'won' | 'lost' = $state('waiting');
</script>

<Game />

{#if state !== 'playing'}
	<Modal>
		{#if state === 'waiting'}
			<p>choose level</p>
		{/if}
	</Modal>
{/if}
