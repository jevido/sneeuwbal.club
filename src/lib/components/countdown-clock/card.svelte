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

<div class="flex flex-col gap-4 md:gap-8">
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
	<p class="-mt-4 text-center">{name}</p>
</div>

<style scoped>
	:root {
		--normal-text-color: hsl(237, 18%, 89%);
		--dark-background: hsl(236, 21%, 26%);
		--very-dark-background: hsl(235, 16%, 14%);
		--reddish: hsl(345, 84%, 66%);
	}

	p {
		color: var(--normal-text-color);
		font-size: 1rem;
		letter-spacing: 3px;
	}

	.wrapper {
		display: flex;
		flex-direction: column;
		border-radius: 7px;
		position: relative;
		box-shadow: 0 10px 5px rgba(0, 0, 0, 0.125);
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
		background-color: var(--dark-background);
		border-bottom: 1px solid var(--very-dark-background);
		filter: brightness(75%);
		border-radius: 7px 7px 0 0;
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
		background-color: var(--dark-background);
		border-radius: 0 0 7px 7px;
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
		color: var(--reddish);
		font-size: 2.5em;
		line-height: 1;
	}

	/* For center corner rounded card */
	:is(.top-front, .top-flip, .bottom-front, .bottom-flip)::before,
	:is(.top-front, .top-flip, .bottom-front, .bottom-flip)::after {
		--size: 7px;
		content: '';
		position: absolute;

		background-color: var(--very-dark-background);
		border-radius: 50%;
		z-index: 1;
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

		p {
			font-size: 1rem;
		}
		:is(.top-front, .top-flip, .bottom-front, .bottom-flip)::before,
		:is(.top-front, .top-flip, .bottom-front, .bottom-flip)::after {
			--size: 12px;
		}
	}
</style>
