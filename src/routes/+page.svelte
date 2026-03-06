<script>
	import HeroSection from '$lib/components/blocks/hero-section.svelte';
	import SnowCanvas from '$lib/components/blocks/snow-canvas.svelte';

	import { oneko } from '$lib/oneko/beer';

	$effect(() => {
		oneko();
	});
</script>

<svelte:head>
	<title>Sneeuwbal Sociëteit</title>
</svelte:head>

<main class="site-shell relative min-h-screen overflow-hidden text-white">
	<SnowCanvas />
	<div
		class="pointer-events-none absolute top-12 -left-24 z-10 h-72 w-72 rounded-full bg-emerald-300/30 blur-3xl"
	></div>
	<div
		class="pointer-events-none absolute -right-20 bottom-20 z-10 h-80 w-80 rounded-full bg-cyan-300/25 blur-3xl"
	></div>
	<div
		class="pointer-events-none absolute top-1/3 left-1/2 z-10 h-64 w-64 -translate-x-1/2 rounded-full bg-amber-200/15 blur-3xl"
	></div>

	<HeroSection />
</main>
