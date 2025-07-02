<script>
	import { SvelteDate } from 'svelte/reactivity';
	import Card from './card.svelte';

	let { target = '2025-11-14T20:00:00.000Z' } = $props();

	let targetDate = new Date(target);
	const currentDate = new SvelteDate(Date.now());

	$effect(() => {
		const interval = setInterval(() => {
			currentDate.setTime(Date.now());
		}, 1000);

		return () => {
			clearInterval(interval);
		};
	});

	let gap = $derived(targetDate.getTime() - currentDate.getTime());
	let time = $derived({
		days: Math.max(0, Math.floor(gap / (1000 * 60 * 60 * 24))),
		hours: Math.max(0, Math.floor((gap / (1000 * 60 * 60)) % 24)),
		minutes: Math.max(0, Math.floor((gap / 1000 / 60) % 60)),
		seconds: Math.max(0, Math.floor((gap / 1000) % 60))
	});
</script>

<div class="z-10 grid w-5/6 grid-cols-2 justify-center gap-4 pb-4 md:w-3/6 md:grid-cols-4 md:gap-8">
	<Card name="Dagen" number={time.days} />
	<Card name="Uren" number={time.hours} />
	<Card name="Minuten" number={time.minutes} />
	<Card name="Seconden" number={time.seconds} />
</div>
