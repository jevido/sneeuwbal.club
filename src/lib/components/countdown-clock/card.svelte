<script>
	let { number = 0, name } = $props();

	let numbers = $derived.by(() => {
		let array = number.toString().split('');
		let res = array.length === 1 ? ['0', ...array] : array;

		return res;
	});

	// svelte-ignore state_referenced_locally
	let topFront = $state(numbers);
	// svelte-ignore state_referenced_locally
	let bottomFront = $state(numbers);
	// svelte-ignore state_referenced_locally
	let topFlip = $state(numbers);
	// svelte-ignore state_referenced_locally
	let bottomFlip = $state(numbers);

	const topFlipStart = () => {
		topFront = numbers;
	};

	const topFlipEnd = () => {
		topFlip = numbers;
	};

	const bottomFlipStart = () => {
		bottomFlip = numbers;
	};

	const bottomFlipEnd = () => {
		bottomFront = numbers;
	};
</script>

<div class="timer-card flex flex-col gap-4 md:gap-6">
	{#key number}
		<div class="wrapper">
			<div class="top-front">
				{#each topFront as num}
					<span>{num}</span>
				{/each}
			</div>
			<div class="top-flip" onanimationstart={topFlipStart} onanimationend={topFlipEnd}>
				{#each topFlip as num}
					<span>{num}</span>
				{/each}
			</div>
			<div class="bottom-front">
				{#each bottomFront as num}
					<span>{num}</span>
				{/each}
			</div>
			<div class="bottom-flip" onanimationend={bottomFlipEnd} onanimationstart={bottomFlipStart}>
				{#each bottomFlip as num}
					<span>{num}</span>
				{/each}
			</div>
		</div>
	{/key}
	<p class="-mt-3 text-center">{name}</p>
</div>

<style scoped>
	.timer-card p {
		color: color-mix(in oklab, white 74%, #84ffe5 26%);
		font-size: 0.72rem;
		font-weight: 700;
		letter-spacing: 0.36em;
		text-transform: uppercase;
	}

	.wrapper {
		display: flex;
		flex-direction: column;
		position: relative;
		border-radius: 16px;
		box-shadow:
			0 22px 40px rgba(1, 8, 16, 0.42),
			inset 0 1px 0 rgba(255, 255, 255, 0.22);
		overflow: hidden;
	}
	.top-front,
	.top-flip,
	.bottom-front,
	.bottom-flip {
		display: flex;
		justify-content: center;
		overflow: hidden;
		padding: 1.25em 0.75em;
		height: 0.5em;
		line-height: 1;
	}

	.top-front,
	.top-flip {
		background: linear-gradient(175deg, rgba(8, 24, 32, 0.9), rgba(12, 49, 56, 0.88));
		border-bottom: 1px solid rgba(186, 255, 241, 0.3);
		filter: brightness(90%);
		border-radius: 16px 16px 0 0;
	}

	.top-flip {
		position: absolute;
		width: 100%;
		top: 0;
		animation: flip-top 0.25s ease-in;
		transform-origin: bottom;
		transform: perspective(200px) rotateX(0);
	}

	@keyframes flip-top {
		100% {
			transform: rotateX(-90deg);
		}
	}

	.bottom-front {
		position: relative;
	}

	.bottom-front,
	.bottom-flip {
		display: flex;
		align-items: flex-end;
		background: linear-gradient(180deg, rgba(2, 17, 23, 0.92), rgba(2, 24, 31, 0.95));
		border-radius: 0 0 16px 16px;
	}

	.bottom-flip {
		position: absolute;
		width: 100%;
		bottom: 0;
		animation: flip-bottom 0.25s ease-out 0.25s;
		transform-origin: top;
		transform: perspective(300px) rotateX(90deg);
	}

	@keyframes flip-bottom {
		100% {
			transform: rotateX(0deg);
		}
	}

	span {
		display: inline-block;
		color: #9cffea;
		text-shadow:
			0 0 12px rgba(122, 255, 233, 0.55),
			0 0 28px rgba(122, 255, 233, 0.15);
		font-size: 2.5em;
		font-weight: 700;
		line-height: 1;
	}

	:is(.top-front, .top-flip, .bottom-front, .bottom-flip)::before,
	:is(.top-front, .top-flip, .bottom-front, .bottom-flip)::after {
		--size: 9px;
		content: '';
		position: absolute;
		background-color: rgba(3, 20, 30, 0.95);
		border-radius: 50%;
		z-index: 1;
		width: var(--size);
		height: var(--size);
	}

	:is(.top-front, .top-flip)::before {
		bottom: 0;
		left: 0;
		transform: translate(-50%, 50%);
	}

	:is(.top-front, .top-flip)::after {
		bottom: 0;
		right: 0;
		transform: translate(50%, 50%);
	}

	:is(.bottom-front, .bottom-flip)::before {
		top: 0;
		left: 0;
		transform: translate(-50%, -50%);
	}
	:is(.bottom-front, .bottom-flip)::after {
		top: 0;
		right: 0;
		transform: translate(50%, -50%);
	}

	@media (min-width: 768px) {
		.top-front,
		.top-flip,
		.bottom-front,
		.bottom-flip {
			padding: 2.5em 1.75em;
			line-height: 1;
		}
		span {
			font-size: 5em;
			height: 1em;
			text-align: center;
			width: 0.75em;
		}
		:is(.top-front, .top-flip, .bottom-front, .bottom-flip)::before,
		:is(.top-front, .top-flip, .bottom-front, .bottom-flip)::after {
			--size: 12px;
		}
	}
</style>
