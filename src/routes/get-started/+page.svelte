<script>
	import { fade, fly } from 'svelte/transition';
	import { goto } from '$app/navigation';
</script>

<div class="container">
	<div class="header" in:fade={{ duration: 400, delay: 200 }}>
		<h1>Get Started</h1>
		<p>Choose your role</p>
	</div>
	
	<div class="split-view">
		<button class="tile buyer" on:click={() => goto('/login?role=buyer')} in:fly={{ y: 50, duration: 600, delay: 400 }}>
			<div class="content">
				<h2>Buyer</h2>
				<p>Find your dream home</p>
			</div>
			<div class="bg-overlay"></div>
		</button>
		
		<button class="tile seller" on:click={() => goto('/login?role=seller')} in:fly={{ y: 50, duration: 600, delay: 500 }}>
			<div class="content">
				<h2>Seller</h2>
				<p>List your property</p>
			</div>
			<div class="bg-overlay"></div>
		</button>
	</div>
</div>

<style>
	.container {
		height: 100vh; /* Fallback */
		height: 100svh;
		width: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		background: #000;
		color: white;
		padding: 2rem 1.5rem;
		box-sizing: border-box;
	}

	.header {
		margin-bottom: 2rem;
		text-align: center;
	}

	h1 {
		font-size: 2rem;
		font-weight: 800;
		margin: 0;
		letter-spacing: -0.03em;
		line-height: 1.1;
	}

	.header p {
		margin: 0.5rem 0 0;
		font-size: 1rem;
		color: rgba(255, 255, 255, 0.6);
	}

	.split-view {
		display: flex;
		flex-direction: column;
		gap: 1rem;
		width: 100%;
		max-width: 500px;
		margin: 0 auto;
	}

	.tile {
		position: relative;
		height: 160px; /* reduced fixed height */
		border: none;
		border-radius: 16px;
		overflow: hidden;
		cursor: pointer;
		display: flex;
		align-items: center;
		padding: 0 2rem;
		text-align: left;
		background: #fff;
		transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
		border: 1px solid rgba(255,255,255,0.1);
	}

	.tile:hover {
		transform: translateY(-2px);
		box-shadow: 0 8px 24px rgba(0,0,0,0.2);
	}

	/* Specific styling for tiles */
	.buyer {
		background: #fff;
		color: black;
	}

	.seller {
		background: #fff;
		color: black;
	}

	.tile:active {
		transform: scale(0.98);
	}

	.content {
		position: relative;
		z-index: 2;
		pointer-events: none;
	}

	.tile h2 {
		font-size: 1.5rem;
		font-weight: 700;
		margin: 0;
		letter-spacing: -0.01em;
	}

	.tile p {
		margin: 0.25rem 0 0;
		font-size: 0.875rem;
		opacity: 0.6;
	}
	
	/* Removed specific seller p style as both are now light */

	.bg-overlay {
		position: absolute;
		inset: 0;
		background-size: cover;
		background-position: center;
		opacity: 0.2;
		transition: opacity 0.3s ease;
	}
	
	.tile:hover .bg-overlay {
		opacity: 0.3;
	}

	@media (min-width: 768px) {
		.split-view {
			flex-direction: row;
		}
		
		h1 {
			font-size: 3.5rem;
		}
	}
</style>
