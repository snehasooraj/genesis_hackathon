<script>
	import Button from '$lib/components/Button.svelte';
	import { goto } from '$app/navigation';
</script>

<section class="app-screen">
	<div class="background-image"></div>
	
	<div class="content-overlay">
		<div class="glass-card">
			<h1>Find Your<br>Perfect Home</h1>
			<p>Discover the most transparent way to browse, tour, and buy your next property.</p>
			
			<Button on:click={() => goto('/login')}>Start Hunting!</Button>
		</div>
	</div>
</section>

<style>
	.app-screen {
		position: relative;
		height: 100vh;
		height: 100svh;
		width: 100%;
		overflow: hidden;
		display: flex;
		flex-direction: column;
		justify-content: flex-end;
		background: #000;
	}

	.background-image {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-image: url('https://images.unsplash.com/photo-1600585154340-be6161a56a0c?auto=format&fit=crop&w=1200&q=80');
		background-size: cover;
		background-position: center;
		transform: scale(1.1); /* Subtle zoom for app feel */
	}

	.content-overlay {
		position: relative;
		z-index: 2;
		padding: 2rem 1.5rem;
		width: 100%;
		box-sizing: border-box;
		background: linear-gradient(to top, rgba(0,0,0,0.8) 0%, rgba(0,0,0,0.4) 60%, transparent 100%);
	}

	.glass-card {
		text-align: left; /* Mobile apps often favor left-aligned or centered; let's try a modern left-align with centered button */
		max-width: 500px;
		margin: 0 auto;
	}

	h1 {
		font-size: 3rem;
		font-weight: 800;
		margin: 0 0 1rem 0;
		line-height: 1;
		letter-spacing: -0.04em;
		color: #ffffff;
	}

	p {
		font-size: 1.125rem;
		color: rgba(255, 255, 255, 0.8);
		margin: 0 0 2.5rem 0;
		line-height: 1.5;
		font-weight: 400;
		max-width: 90%;
	}



	/* Subtle entrance animation */
	.glass-card {
		animation: slideUp 0.8s cubic-bezier(0.2, 0.8, 0.2, 1) forwards;
	}

	@keyframes slideUp {
		from { opacity: 0; transform: translateY(40px); }
		to { opacity: 1; transform: translateY(0); }
	}

	@media (min-width: 768px) {
		h1 {
			font-size: 4rem;
		}
		
		.content-overlay {
			padding: 4rem;
		}
	}
</style>