<script>
	// todo: These snowflakes are hideous
	// import { onMount } from 'svelte';

	// let canvas;
	// let ctx;
	// let width, height;
	// let snowflakes = [];

	// onMount(() => {
	// 	ctx = canvas.getContext('2d');
	// 	resize();
	// 	window.addEventListener('resize', resize);

	// 	for (let i = 0; i < 150; i++) {
	// 		snowflakes.push({
	// 			x: Math.random() * width,
	// 			y: Math.random() * height,
	// 			r: Math.random() * 3 + 1,
	// 			d: Math.random() * 1
	// 		});
	// 	}

	// 	animate();
	// });

	// function resize() {
	// 	width = canvas.width = window.innerWidth;
	// 	height = canvas.height = window.innerHeight;
	// }

	// function animate() {
	// 	ctx.clearRect(0, 0, width, height);
	// 	ctx.fillStyle = 'white';
	// 	ctx.beginPath();
	// 	for (let flake of snowflakes) {
	// 		ctx.moveTo(flake.x, flake.y);
	// 		ctx.arc(flake.x, flake.y, flake.r, 0, Math.PI * 2);
	// 	}
	// 	ctx.fill();

	// 	for (let flake of snowflakes) {
	// 		flake.y += flake.d + 1;
	// 		if (flake.y > height) {
	// 			flake.y = 0;
	// 			flake.x = Math.random() * width;
	// 		}
	// 	}

	// 	requestAnimationFrame(animate);
	// }
</script>

<div class="fixed inset-0 z-0">
	<video class="h-full w-full object-cover" autoplay muted loop>
		<source src="/yeah.mp4" type="video/mp4" />
		Your browser sucks.
	</video>
	<div class="fixed inset-0 z-10 bg-gradient-to-b from-black/70 to-black/30"></div>

	<!-- snow canvas -->
	<!-- <canvas bind:this={canvas} class="pointer-events-none fixed inset-0 z-20"></canvas> -->
</div>
