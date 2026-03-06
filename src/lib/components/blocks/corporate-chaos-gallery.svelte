<script>
	// Replace these URLs with your own images (20 slots).
	const galleryImages = [
		'https://picsum.photos/seed/sleaze-01/1400/1000',
		'https://picsum.photos/seed/sleaze-02/1400/1000',
		'https://picsum.photos/seed/sleaze-03/1400/1000',
		'https://picsum.photos/seed/sleaze-04/1400/1000',
		'https://picsum.photos/seed/sleaze-05/1400/1000',
		'https://picsum.photos/seed/sleaze-06/1400/1000',
		'https://picsum.photos/seed/sleaze-07/1400/1000',
		'https://picsum.photos/seed/sleaze-08/1400/1000',
		'https://picsum.photos/seed/sleaze-09/1400/1000',
		'https://picsum.photos/seed/sleaze-10/1400/1000',
		'https://picsum.photos/seed/sleaze-11/1400/1000',
		'https://picsum.photos/seed/sleaze-12/1400/1000',
		'https://picsum.photos/seed/sleaze-13/1400/1000',
		'https://picsum.photos/seed/sleaze-14/1400/1000',
		'https://picsum.photos/seed/sleaze-15/1400/1000',
		'https://picsum.photos/seed/sleaze-16/1400/1000',
		'https://picsum.photos/seed/sleaze-17/1400/1000',
		'https://picsum.photos/seed/sleaze-18/1400/1000',
		'https://picsum.photos/seed/sleaze-19/1400/1000',
		'https://picsum.photos/seed/sleaze-20/1400/1000'
	];

	const panels = [
		{ x: -3, y: -2, w: 16, h: 24, r: -3.4, z: 7 },
		{ x: 9, y: -1, w: 16, h: 31, r: 2.6, z: 6 },
		{ x: 21, y: -3, w: 22, h: 29, r: -3.8, z: 9 },
		{ x: 38, y: -2, w: 19, h: 27, r: 2.2, z: 5 },
		{ x: 54, y: -1, w: 18, h: 25, r: -2.5, z: 8 },
		{ x: 69, y: -2, w: 16, h: 33, r: 2.8, z: 4 },
		{ x: 82, y: -1, w: 18, h: 27, r: -3.1, z: 7 },
		{ x: -2, y: 19, w: 16, h: 31, r: 1.9, z: 5 },
		{ x: 10, y: 25, w: 16, h: 25, r: -2.8, z: 10 },
		{ x: 22, y: 23, w: 16, h: 27, r: 3.3, z: 6 },
		{ x: 34, y: 20, w: 39, h: 31, r: -2.3, z: 11 },
		{ x: 68, y: 26, w: 18, h: 26, r: 2.5, z: 5 },
		{ x: 83, y: 21, w: 18, h: 30, r: -2.1, z: 9 },
		{ x: -1, y: 48, w: 31, h: 27, r: 1.4, z: 7 },
		{ x: 23, y: 49, w: 18, h: 27, r: -1.8, z: 8 },
		{ x: 38, y: 49, w: 28, h: 21, r: 2.2, z: 12 },
		{ x: 61, y: 49, w: 25, h: 23, r: -2.5, z: 6 },
		{ x: 82, y: 50, w: 18, h: 27, r: 1.7, z: 10 },
		{ x: 39, y: 67, w: 24, h: 23, r: -1.6, z: 8 },
		{ x: 58, y: 69, w: 29, h: 21, r: 2.1, z: 9 }
	];

	const cuts = [
		'polygon(2% 0, 100% 1%, 98% 100%, 0 97%)',
		'polygon(0 2%, 98% 0, 100% 97%, 3% 100%)',
		'polygon(1% 0, 100% 3%, 97% 99%, 0 95%)',
		'polygon(0 0, 99% 2%, 100% 100%, 2% 98%)',
		'polygon(3% 1%, 100% 0, 98% 98%, 0 100%)'
	];

	const panelStyle = (panel, index) =>
		`left:${panel.x}%;top:${panel.y}%;width:${panel.w}%;height:${panel.h}%;z-index:${panel.z};--rot:${panel.r}deg;--cut:${cuts[index % cuts.length]};`;
</script>

<section class="relative z-20 pb-24 md:pb-32">
	<div class="mx-auto w-[min(1200px,94vw)]">
		<div
			class="mb-6 rounded-[1.4rem] border border-red-200/20 bg-black/45 p-5 shadow-[0_22px_55px_rgba(0,0,0,0.5)] backdrop-blur-lg md:mb-8 md:p-7"
		>
			<p class="text-xs font-semibold tracking-[0.24em] text-amber-100 uppercase">Fotowall</p>
			<h3 class="font-display mt-2 text-3xl font-extrabold md:text-5xl">
				Plak er beelden in. Maak er een puinhoop van.
			</h3>
		</div>

		<div class="comic-wall">
			{#each panels as panel, index}
				<figure class="comic-panel" style={panelStyle(panel, index)}>
					<img
						src={galleryImages[index % galleryImages.length]}
						alt={`Gallery image ${index + 1}`}
						loading="lazy"
					/>
				</figure>
			{/each}
		</div>
	</div>
</section>

<style>
	.comic-wall {
		position: relative;
		aspect-ratio: 16 / 9;
		background: #000;
		border: 5px solid #050505;
		overflow: hidden;
	}

	.comic-panel {
		position: absolute;
		padding: 0.42rem;
		background: #020202;
		border: 2px solid rgb(0 0 0 / 0.95);
		clip-path: var(--cut);
		transform: rotate(var(--rot));
		overflow: hidden;
		box-shadow:
			0 14px 28px rgb(0 0 0 / 0.46),
			0 0 0 1px rgb(255 255 255 / 0.1) inset;
		transition:
			transform 180ms ease,
			z-index 0ms linear 180ms;
	}

	.comic-panel:hover {
		transform: rotate(0deg) scale(1.03);
		z-index: 8;
		transition:
			transform 180ms ease,
			z-index 0ms linear;
	}

	.comic-panel img {
		width: 100%;
		height: 100%;
		display: block;
		object-fit: cover;
		object-position: center;
		clip-path: var(--cut);
		filter: contrast(1.08) saturate(1.2);
		transform: scale(1.05);
	}

	@media (max-width: 900px) {
		.comic-wall {
			aspect-ratio: 9 / 16;
		}
	}
</style>
