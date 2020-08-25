<script>
	import Slider from './Slider.svelte';
	import Controller from './Controller.svelte';
	import { round } from '../stores/gameStore';
	export let timeFromUser;
	export let breakTimeFromUser;
	export let roundsToBreakFromUser;

	const resetTimer = () => {
		return timeFromUser;
	};
	let timeRemaining = resetTimer();

	const setBreakDuration = () => {
		return breakTimeFromUser;
	};

	const initiateBreakTimer = () => {
		return roundsToBreakFromUser * timeFromUser;
	};
	let timeToBreak = initiateBreakTimer();

	const setBreakTimer = () => {
		return roundsToBreakFromUser * timeFromUser + setBreakDuration();
	};

	let isPaused = false;
	let isBreak = false;

	const setMax = () => {
		if (isBreak) {
			return breakTimeFromUser;
		}
		return timeFromUser;
	};
	let max = setMax();

	//const audio = new Audio('https://www.soundjay.com/button/beep-01a.mp3');

	const oneMin = 60;
	const tenSec = 10;

	const formatTime = (timeRemaining) => {
		const minutes = Math.floor(timeRemaining / 60);
		const seconds = timeRemaining % 60;
		const zeroPaddedSeconds = seconds.toString().padStart(2, '0');
		return `${minutes}:${zeroPaddedSeconds}`;
	};

	const togglePause = () => (isPaused = !isPaused);
	const toggleBreak = () => (isBreak = !isBreak);
	const reduceTime = () => {
		// if (timeRemaining == oneMin) {
		// 	audio.play();
		// }
		// if (timeRemaining > 0 && timeRemaining < tenSec) {
		// 	audio.play();
		// }
		if (timeToBreak === 0) {
			isBreak = true;
			timeRemaining = setBreakDuration();
			timeToBreak = setBreakTimer();
			clearInterval(interval);
			interval = setInterval(reduceTime, 1000);
		}

		if (timeRemaining === 0) {
			isBreak = false;
			round.increment();
			timeRemaining = resetTimer();
			clearInterval(interval);
			interval = setInterval(reduceTime, 1000);
		}
		if (!isPaused) {
			timeRemaining = Math.max(0, timeRemaining - 1);
			timeToBreak = Math.max(0, timeToBreak - 1);
		}
	};
	let interval = setInterval(reduceTime, 1000);
</script>

<style>
	div {
		color: #dbe3d0;
		/* color: #35654d; */
		display: flex;
		width: 100%;
		justify-content: center;
		flex-direction: column;
		align-items: center;
	}

	p {
		font-size: 14rem;
		font-weight: 100;
		margin: 0;
		font-variant-numeric: tabular-nums;
	}
</style>

<div>
	<p>{formatTime(timeRemaining)}</p>
	<Slider bind:timeRemaining max={setMax()} />
	<Controller {isPaused} {togglePause} />
</div>
