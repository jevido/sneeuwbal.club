<script>
	import { onMount } from 'svelte';
	import { gsap } from 'gsap';
	import Logo from '$lib/logo.svelte';
	import CountdownTimer from '$lib/components/blocks/countdown-timer.svelte';

	let startHidden = $state(true);

	onMount(() => {
		startHidden = false;

		gsap.from('.throw-me, .throw-me *', {
			opacity: 0,
			y: 56,
			rotate: 4,
			stagger: 0.08,
			duration: 0.85,
			ease: 'power3.out'
		});
	});
</script>

<div class="relative z-20 flex min-h-screen items-center justify-center px-4 py-8 md:px-8">
	<section>
		<h1 class="text-center">
			<Logo
				class="throw-me relative z-20 mx-auto size-64 drop-shadow-[0_16px_32px_rgba(0,0,0,0.45)] md:size-[24rem] {startHidden &&
					'invisible'}"
			/>
		</h1>

		<div class="throw-me">
			<CountdownTimer />
		</div>

		<div class="throw-me mt-2 text-center">
			<p
				class="font-display bg-gradient-to-r from-emerald-200 via-cyan-100 to-amber-100 bg-clip-text text-3xl font-extrabold tracking-[0.18em] text-transparent uppercase md:text-5xl"
			>
				14 - 16 November
			</p>
			<p class="mt-2 text-sm tracking-[0.4em] text-white/75 uppercase md:text-base">
				Kouwe lucht. Warm bier.
			</p>
		</div>
	</section>
</div>
