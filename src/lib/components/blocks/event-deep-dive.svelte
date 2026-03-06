<script>
	import { onMount } from 'svelte';

	const program = [
		{
			label: 'Vrijdag',
			title: 'Aankomst + Eerste Tap',
			description:
				'Inchecken, kamers claimen, eerste ronde op temperatuur en direct een zachte landing in Sneeuwbal-stijl.'
		},
		{
			label: 'Zaterdag',
			title: 'Buiten Chaos + Binnen Kroeg',
			description:
				'Actieve dag met team challenges, koude lucht, hete snacks en een ochtendprogramma.'
		},
		{
			label: 'Zondag',
			title: 'Slow Exit + Legendarische Ragout',
			description:
				'Brakkies an wakker worden, groepsfoto en een waardige uitloop richting de echte wereld.'
		}
	];

	const highlights = [
		'Live DJ set met requests',
		'Onderling casino',
		'De proeverij',
		'Ding 4',
		'Bier',
		'Finale van Chaos pong'
	];

	const practical = [
		{ key: 'Locatie', value: 'Wordt binnenkort naar gezocht' },
		{ key: 'Dresscode', value: 'Hot & Cold, kom verkleed' },
		{ key: 'Meenemen', value: 'Je favoriete drank & je beste duim(en)' },
		{ key: 'Slaap', value: 'We barsten van de slaapzakjes' }
	];

	const regiemVoices = [
		{
			name: 'Hakker',
			role: '',
			quote: ''
		},
		{
			name: 'Makker',
			role: 'temu sinterklaas',
			quote: 'Piek vroeg, drink hard, dan wil iedereen dat je rustiger doet, en ga je niet te hard'
		},
		{
			name: 'Stakker',
			role: '',
			quote: ''
		},
		{
			name: 'Rakker',
			role: 'Aanrand piet',
			quote: 'Neem je moeder mee, hebben we allebei een goeie avond'
		}
	];

	let shuffledVoices = $state(regiemVoices);
	let currentVoice = $state(0);
	let touchStartX = 0;

	onMount(() => {
		shuffledVoices = [...regiemVoices]
			.map((voice) => ({ voice, sort: Math.random() }))
			.sort((a, b) => a.sort - b.sort)
			.map(({ voice }) => voice);
	});

	const previousVoice = () => {
		currentVoice = (currentVoice - 1 + shuffledVoices.length) % shuffledVoices.length;
	};

	const nextVoice = () => {
		currentVoice = (currentVoice + 1) % shuffledVoices.length;
	};

	const setVoice = (index) => {
		currentVoice = index;
	};

	const onSwipeStart = (event) => {
		touchStartX = event.changedTouches[0]?.clientX ?? 0;
	};

	const onSwipeEnd = (event) => {
		const touchEndX = event.changedTouches[0]?.clientX ?? touchStartX;
		const delta = touchEndX - touchStartX;
		if (Math.abs(delta) < 35) return;
		if (delta > 0) previousVoice();
		else nextVoice();
	};
</script>

<section class="relative z-20 pt-8 pb-24 md:pt-14 md:pb-32">
	<div class="mx-auto w-[min(1100px,92vw)]">
		<div
			class="mb-8 rounded-[1.8rem] border border-white/30 bg-black/35 p-6 shadow-[0_24px_80px_rgba(0,0,0,0.45)] backdrop-blur-xl md:mb-12 md:p-10"
		>
			<p class="mb-3 text-xs font-semibold tracking-[0.24em] text-cyan-100 uppercase">
				Wat Gaan We Doen
			</p>
			<h2 class="font-display text-3xl leading-tight font-extrabold md:text-5xl">
				10 jaar leven in de barre bunk beds
			</h2>
			<p class="mt-4 max-w-3xl text-sm leading-relaxed text-white/82 md:text-base">
				Het is nou al een tijdje bezig, en we zijn gegroeid van 7 vage gasten die saucijzenbroodjes maken. Naar een stuk of wat.
				Als me dat godver geen reden is om wat te doen. Dan weet ik het ook niet meer.
				Sterker nog, ik weet het al niet meer.
				Als je nou gewoon ff naar beneden scrolled. Wordt dit toch wel minder ongemakkelijk voor ons beide.
			</p>
		</div>

		<div class="grid gap-6 md:grid-cols-3">
			{#each program as item}
				<article
					class="rounded-3xl border border-cyan-100/25 bg-gradient-to-b from-cyan-100/10 to-emerald-200/8 p-5 shadow-[0_18px_48px_rgba(0,0,0,0.35)] backdrop-blur-lg md:p-6"
				>
					<p class="mb-3 text-xs font-semibold tracking-[0.22em] text-cyan-100 uppercase">
						{item.label}
					</p>
					<h3 class="font-display text-2xl leading-tight font-bold">{item.title}</h3>
					<p class="mt-3 text-sm leading-relaxed text-white/78">{item.description}</p>
				</article>
			{/each}
		</div>

		<div class="mt-6 grid gap-6 md:mt-8 md:grid-cols-[1.25fr_0.75fr]">
			<section
				class="rounded-3xl border border-amber-100/25 bg-gradient-to-br from-amber-100/10 to-cyan-100/8 p-6 shadow-[0_20px_52px_rgba(0,0,0,0.35)] backdrop-blur-lg md:p-8"
			>
				<p class="mb-4 text-xs font-semibold tracking-[0.22em] text-amber-100 uppercase">
					Highlights
				</p>
				<ul class="grid gap-3 md:grid-cols-2">
					{#each highlights as point}
						<li
							class="rounded-xl border border-white/20 bg-white/6 px-4 py-3 text-sm text-white/86"
						>
							{point}
						</li>
					{/each}
				</ul>
			</section>

			<section
				class="rounded-3xl border border-emerald-100/25 bg-gradient-to-b from-emerald-100/10 to-cyan-100/8 p-6 shadow-[0_20px_52px_rgba(0,0,0,0.35)] backdrop-blur-lg md:p-8"
			>
				<p class="mb-4 text-xs font-semibold tracking-[0.22em] text-emerald-100 uppercase">
					Praktisch
				</p>
				<div class="space-y-3">
					{#each practical as row}
						<div class="rounded-xl border border-white/18 bg-white/6 px-4 py-3">
							<p class="text-[0.7rem] font-semibold tracking-[0.18em] text-white/65 uppercase">
								{row.key}
							</p>
							<p class="mt-1 text-sm text-white/88">{row.value}</p>
						</div>
					{/each}
				</div>
			</section>
		</div>

		<section
			class="mt-6 rounded-3xl border border-cyan-100/25 bg-gradient-to-br from-cyan-100/10 to-emerald-100/10 p-6 shadow-[0_20px_52px_rgba(0,0,0,0.35)] backdrop-blur-lg md:mt-8 md:p-8"
		>
			<p class="mb-2 text-xs font-semibold tracking-[0.22em] text-cyan-100 uppercase">
				Het Regiem 2026
			</p>
			<h3 class="font-display text-2xl font-bold md:text-3xl">
				Het regiem wil je wat advies geven voor dit jaar
			</h3>
			<div class="mt-3 flex items-center justify-between gap-3">
				<div class="hidden w-full justify-between gap-2 sm:flex">
					<button
						type="button"
						onclick={previousVoice}
						class="rounded-full border border-white/30 bg-black/20 px-4 py-2 text-xs tracking-[0.15em] text-white/88 uppercase transition hover:bg-white/15"
						aria-label="Vorige quote"
					>
						niet verder
					</button>
					<button
						type="button"
						onclick={nextVoice}
						class="rounded-full border border-white/30 bg-black/20 px-4 py-2 text-xs tracking-[0.15em] text-white/88 uppercase transition hover:bg-white/15"
						aria-label="Volgende quote"
					>
						Verder
					</button>
				</div>
			</div>

			<div
				class="mt-4 overflow-hidden rounded-2xl border border-white/22 bg-black/25"
				ontouchstart={onSwipeStart}
				ontouchend={onSwipeEnd}
			>
				<div
					class="flex transition-transform duration-500 ease-out"
					style={`transform: translateX(-${currentVoice * 100}%);`}
				>
					{#each shuffledVoices as voice}
						<article class="min-w-full p-6 md:p-8">
							<p class="text-[0.7rem] font-semibold tracking-[0.2em] text-cyan-100 uppercase">
								{voice.name} · {voice.role}
							</p>
							<p class="mt-4 max-w-3xl text-base leading-relaxed text-white/92 md:text-xl">
								{voice.quote}
							</p>
						</article>
					{/each}
				</div>
			</div>

			<div class="mt-4 flex items-center justify-center gap-2">
				{#each shuffledVoices as _, index}
					<button
						type="button"
						onclick={() => setVoice(index)}
						class={`h-2.5 rounded-full transition ${
							currentVoice === index ? 'w-8 bg-cyan-200' : 'w-2.5 bg-white/35 hover:bg-white/60'
						}`}
						aria-label={`Ga naar quote ${index + 1}`}
					></button>
				{/each}
			</div>
		</section>
	</div>
</section>
