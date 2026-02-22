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

<main
	class="relative min-h-screen overflow-hidden bg-gradient-to-b from-slate-900 to-slate-800 text-white"
>
	<SnowCanvas />

	<HeroSection />
</main>

