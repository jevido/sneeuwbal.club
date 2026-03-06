<script>
	import { onMount } from 'svelte';
	import EventDeepDive from '$lib/components/blocks/event-deep-dive.svelte';
	import HeroSection from '$lib/components/blocks/hero-section.svelte';
	import SnowCanvas from '$lib/components/blocks/snow-canvas.svelte';

	import { oneko } from '$lib/oneko/beer';
	const secretSequence = [
		'ArrowUp',
		'ArrowUp',
		'ArrowDown',
		'ArrowDown',
		'ArrowLeft',
		'ArrowRight',
		'ArrowLeft',
		'ArrowRight',
		'b',
		'a'
	];
	let secretIndex = 0;
	let easterEggActive = $state(false);
	let easterEggTimeout = null;
	let spriteTicker = $state(0);
	let spriteSwarm = $state([]);
	let weirdWord = $state('');

	const spriteStep = 32;
	const weirdSpriteSets = {
		play: [
			[0, 1],
			[0, 2]
		],
		scratchSelf: [
			[-5, 0],
			[-6, 0],
			[-7, 0]
		],
		sleeping: [
			[-2, 0],
			[-2, -1]
		],
		NE: [
			[0, -2],
			[0, -3]
		],
		SW: [
			[-5, -3],
			[-6, -1]
		],
		alert: [[-7, -3]]
	};
	const weirdAnimations = Object.keys(weirdSpriteSets);

	const weirdWords = [
		'BIERPLEZIER OP DE VLIER',
		'FERMENTATIE GENERATIE',
		'GIST OP DE LIJST',
		'MOUT IN HET WOUT',
		'SCHUIM IS PRUIM',
		'DE KAT HERINNERT ZICH ALLES'
	];

	$effect(() => {
		oneko();
	});

	$effect(() => {
		if (!easterEggActive) {
			spriteSwarm = [];
			weirdWord = '';
			return;
		}

		weirdWord = weirdWords[Math.floor(Math.random() * weirdWords.length)];
		spriteSwarm = Array.from({ length: 20 }, (_, index) => ({
			id: `${Date.now()}-${index}`,
			xStart: -20 - Math.random() * 45,
			yStart: Math.random() * 95,
			size: 2.9 + Math.random() * 5.4,
			delay: -(Math.random() * 3.2),
			duration: 2.1 + Math.random() * 4.1,
			tilt: -18 + Math.random() * 36,
			drift: 78 + Math.random() * 65,
			flicker: Math.random() * 0.45,
			hue: -25 + Math.random() * 95,
			anim: weirdAnimations[Math.floor(Math.random() * weirdAnimations.length)],
			frame: 0
		}));
	});

	$effect(() => {
		if (!easterEggActive) return;

		const interval = setInterval(() => {
			spriteTicker += 1;
		}, 100);

		return () => clearInterval(interval);
	});

	$effect(() => {
		if (!easterEggActive || spriteSwarm.length === 0) return;
		spriteTicker;

		spriteSwarm = spriteSwarm.map((sprite) => {
			const maybeSwitch = Math.random() < 0.12;
			const nextAnim = maybeSwitch
				? weirdAnimations[Math.floor(Math.random() * weirdAnimations.length)]
				: sprite.anim;
			const frames = weirdSpriteSets[nextAnim];
			const nextFrame = (sprite.frame + 1) % frames.length;
			const framePosition = frames[nextFrame];

			return {
				...sprite,
				anim: nextAnim,
				frame: nextFrame,
				bgX: framePosition[0] * spriteStep,
				bgY: framePosition[1] * spriteStep
			};
		});
	});

	onMount(() => {
		const onKeyDown = (event) => {
			const key = event.key.length === 1 ? event.key.toLowerCase() : event.key;

			if (key === secretSequence[secretIndex]) {
				secretIndex += 1;

				if (secretIndex === secretSequence.length) {
					easterEggActive = true;
					secretIndex = 0;
					clearTimeout(easterEggTimeout);
					easterEggTimeout = setTimeout(() => {
						easterEggActive = false;
					}, 8000);
				}

				return;
			}

			secretIndex = key === secretSequence[0] ? 1 : 0;
		};

		window.addEventListener('keydown', onKeyDown);
		return () => {
			clearTimeout(easterEggTimeout);
			window.removeEventListener('keydown', onKeyDown);
		};
	});
</script>

<svelte:head>
	<title>Sneeuwbal Sociëteit</title>
</svelte:head>

<main
	class:secret-dimension={easterEggActive}
	class="site-shell relative min-h-screen overflow-x-hidden text-white"
>
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
	<EventDeepDive />

	{#if easterEggActive}
		<div class="secret-realm fixed inset-0 z-50">
			<div class="secret-noise"></div>
			<div class="secret-vignette"></div>
			<div class="secret-swarm">
				{#each spriteSwarm as sprite (sprite.id)}
					<div
						class="secret-sprite"
						style={`--x-start: ${sprite.xStart}vw; --y-start: ${sprite.yStart}vh; --size: ${sprite.size}rem; --delay: ${sprite.delay}s; --duration: ${sprite.duration}s; --tilt: ${sprite.tilt}deg; --drift: ${sprite.drift}vw; --flicker: ${sprite.flicker}s; --hue: ${sprite.hue}deg; --bg-x: ${sprite.bgX ?? 0}px; --bg-y: ${sprite.bgY ?? 0}px;`}
					></div>
				{/each}
			</div>
			<p class="secret-chant">{weirdWord}</p>
		</div>
	{/if}
</main>

<style>
	.secret-realm {
		pointer-events: none;
		isolation: isolate;
	}

	.secret-noise,
	.secret-vignette {
		position: absolute;
		inset: 0;
	}

	.secret-noise {
		mix-blend-mode: soft-light;
		background:
			repeating-linear-gradient(0deg, rgba(255, 255, 255, 0.1) 0 2px, rgba(0, 0, 0, 0.02) 2px 4px),
			repeating-linear-gradient(
				90deg,
				rgba(10, 251, 231, 0.08) 0 1px,
				transparent 1px 3px,
				rgba(255, 170, 65, 0.09) 3px 4px
			);
		animation: noise-shift 0.15s steps(2, end) infinite;
	}

	.secret-vignette {
		background: radial-gradient(circle, transparent 28%, rgba(0, 0, 0, 0.58) 100%);
	}

	.secret-swarm {
		position: absolute;
		inset: 0;
		overflow: hidden;
	}

	.secret-sprite {
		position: absolute;
		left: 0;
		top: 0;
		width: var(--size);
		height: var(--size);
		transform: translate3d(var(--x-start), var(--y-start), 0);
		background-image: url('/130px-Beer_detail.png');
		background-position: var(--bg-x) var(--bg-y);
		background-repeat: no-repeat;
		background-size: auto;
		filter: hue-rotate(var(--hue)) saturate(1.35) contrast(1.18)
			drop-shadow(0 0 16px rgba(0, 255, 224, 0.42)) drop-shadow(0 0 28px rgba(255, 204, 74, 0.2));
		image-rendering: pixelated;
		opacity: 0;
		animation:
			secret-chaos var(--duration) linear var(--delay) infinite,
			secret-flicker calc(0.14s + var(--flicker)) steps(2, end) infinite;
		will-change: transform, opacity, filter;
	}

	.secret-chant {
		position: absolute;
		top: 8vh;
		left: 50%;
		transform: translateX(-50%);
		color: #f8ffad;
		font-size: clamp(1rem, 4vw, 2.6rem);
		font-weight: 800;
		letter-spacing: 0.28em;
		text-transform: uppercase;
		text-shadow:
			0 0 8px rgba(248, 255, 173, 0.7),
			0 0 20px rgba(0, 255, 216, 0.35);
		mix-blend-mode: screen;
		animation: chant-wobble 0.25s steps(2, end) infinite;
	}

	.secret-dimension {
		animation: realm-warp 0.38s steps(2, end) infinite;
	}

	@keyframes secret-chaos {
		0% {
			transform: translate3d(var(--x-start), var(--y-start), 0) rotate(0deg) scale(0.7);
			opacity: 0;
		}
		8% {
			opacity: 1;
		}
		28% {
			transform: translate3d(calc(var(--x-start) + 35vw), calc(var(--y-start) - 24vh), 0)
				rotate(var(--tilt)) scale(1.35);
		}
		52% {
			transform: translate3d(calc(var(--x-start) + var(--drift)), calc(var(--y-start) + 10vh), 0)
				rotate(calc(var(--tilt) * -1.35)) scale(0.94);
		}
		74% {
			transform: translate3d(
					calc(var(--x-start) + var(--drift) * 0.8),
					calc(var(--y-start) + 36vh),
					0
				)
				rotate(calc(var(--tilt) * 1.9)) scale(1.2);
		}
		100% {
			transform: translate3d(calc(var(--x-start) + 116vw), calc(var(--y-start) + 4vh), 0)
				rotate(calc(var(--tilt) * -0.8)) scale(0.7);
			opacity: 0;
		}
	}

	@keyframes secret-flicker {
		0%,
		100% {
			opacity: 0.25;
		}
		50% {
			opacity: 0.96;
		}
	}

	@keyframes noise-shift {
		0% {
			transform: translate(0, 0);
		}
		50% {
			transform: translate(2px, -2px);
		}
		100% {
			transform: translate(-1px, 1px);
		}
	}

	@keyframes chant-wobble {
		0% {
			transform: translateX(-50%) skewX(0deg);
		}
		25% {
			transform: translateX(calc(-50% - 7px)) skewX(8deg);
		}
		50% {
			transform: translateX(calc(-50% + 10px)) skewX(-8deg);
		}
		100% {
			transform: translateX(-50%) skewX(0deg);
		}
	}

	@keyframes realm-warp {
		0% {
			filter: hue-rotate(0deg) contrast(1) saturate(1);
		}
		50% {
			filter: hue-rotate(45deg) contrast(1.2) saturate(1.35);
		}
		100% {
			filter: hue-rotate(-18deg) contrast(0.94) saturate(0.86);
		}
	}
</style>
