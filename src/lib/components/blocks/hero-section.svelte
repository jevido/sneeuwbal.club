<script>
	import gsap from 'gsap';
	import Logo from '$lib/logo.svelte';
	import CountdownTimer from '$lib/components/blocks/countdown-timer.svelte';

	let logoElement = $state();
	let startHidden = $state(true);

	$effect(() => {
		startHidden = false;

		gsap.from('.throw-me, .throw-me *', {
			opacity: 0,
			y: -50,
			rotate: 45,
			stagger: 0.1,
			duration: 1,
			ease: 'back.out(1.7)'
		});
	});
</script>

<div class="z-20 mt-8 flex h-screen flex-col items-center md:justify-center">
	<h1 class="text-7xl font-bold tracking-widest">
		<Logo class="throw-me relative z-20 size-64 md:size-96 {startHidden && 'invisible'}" />
	</h1>

	<CountdownTimer />

	<p
		class="throw-me z-20 {startHidden &&
			'invisible'} bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-400 bg-clip-text text-4xl font-bold text-transparent"
	>
		14 - 16 November
	</p>
	<p class="throw-me z-20 {startHidden && 'invisible'} text-muted-foreground z-20">
		De rest komt nog
	</p>
</div>
