<script lang="ts">
	import { getContext } from 'svelte';
	import { isInactive } from './stores/metaStore';
	import Timer from './Timer/Timer.svelte';
	import BlindViewer from './BlindsViewer/BlindViewer.svelte';
	import data from './data';
	import Fullscreen from './Fullscreen.svelte';

	const timeFromUser = data.timePerRound;
	const breakTimeFromUser = data.timePerBreak;
	const roundsToBreakFromUser = data.roundsPerBreak;
</script>

<style>
	main {
		background-color: #333;
		transition: background-color 0.4s ease;
		text-align: center;
		display: flex;
		align-items: center;
		justify-content: space-evenly;
		flex-direction: column;
		width: 100vw;
		height: 100vh;
	}

	.inactive {
		cursor: none;
		background-color: black;
	}
</style>

<!-- <svelte:window on:keydown={handleKeydown} /> -->
<main id="mainContainer" class={$isInactive ? 'inactive' : ''}>
	<Fullscreen />
	<Timer {timeFromUser} {breakTimeFromUser} {roundsToBreakFromUser} />
	<BlindViewer />
</main>
