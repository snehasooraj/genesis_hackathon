<script>
	import { page } from '$app/stores';
    import { goto } from '$app/navigation';
	import { fade, fly } from 'svelte/transition';
	import Button from '$lib/components/Button.svelte';

	// Extract params
	$: title = $page.url.searchParams.get('title') || 'Luxury Property';
	$: price = $page.url.searchParams.get('price') || '1.0 Cr';
	$: location = $page.url.searchParams.get('location') || 'Hyderabad';
	$: image = $page.url.searchParams.get('image') || '';
	$: bhk = $page.url.searchParams.get('bhk') || '3 BHK';
	$: area = $page.url.searchParams.get('area') || '2000 sqft';
	
	// Mock average bid logic (e.g. 5% less than asking price)
	$: avgBid = calculateAvgBid(price);

	function calculateAvgBid(priceStr) {
		// Very rough mock parsing
		let num = parseFloat(priceStr.split(' ')[0]);
		let unit = priceStr.split(' ')[1];
		if (!num) return priceStr;
		
		let avg = (num * 0.95).toFixed(2);
		return `${avg} ${unit}`;
	}

	function handleMakeOffer() {
		const params = $page.url.searchParams.toString();
		goto(`/make-bid?${params}`);
	}

    function handleBack() {
        history.back();
    }
</script>

<div class="bid-screen">
	<!-- Hero Image -->
	<div class="hero-section">
		<div class="image-container">
			<img src={image} alt={title} />
			<div class="overlay"></div>
			<button class="back-btn" on:click={handleBack}>
				<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 12H5"/><path d="M12 19l-7-7 7-7"/></svg>
			</button>
		</div>
	</div>

	<!-- Content -->
	<div class="content-body" in:fly={{ y: 20, duration: 500, delay: 200 }}>
		<div class="header-info">
			<h1>{title}</h1>
			<p class="location-text">
				<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path><circle cx="12" cy="10" r="3"></circle></svg>
				{location}
			</p>
			
			<div class="specs-row">
				<div class="spec-item">
					<span class="label">Config</span>
					<span class="value">{bhk}</span>
				</div>
				<div class="divider"></div>
				<div class="spec-item">
					<span class="label">Area</span>
					<span class="value">{area}</span>
				</div>
				<div class="divider"></div>
				<div class="spec-item">
					<span class="label">Monthly Rent</span>
					<span class="value">₹ {price}</span>
				</div>
			</div>
		</div>

		<div class="bid-stats-card">
			<div class="stat-title">Market Activity</div>
			<div class="avg-bid-row">
				<div class="avg-label">Average Offer</div>
				<div class="avg-value">₹ {avgBid}</div>
			</div>
			<div class="bid-bar">
				<div class="bar-fill" style="width: 70%"></div>
			</div>
			<p class="stat-desc">7 other customers have made offers recently.</p>
		</div>

		<div class="details-section">
			<h3>About Property</h3>
			<p>Experience luxury living in this beautifully designed home. Featuring modern amenities, spacious interiors, and a prime location that offers both tranquility and connectivity.</p>
		</div>
	</div>

	<!-- Bottom Action -->
	<div class="bottom-action">
		<Button on:click={handleMakeOffer}>Make Offer</Button>
	</div>
</div>

<style>
	.bid-screen {
		min-height: 100vh;
		min-height: 100svh;
		background: #000;
		color: white;
		display: flex;
		flex-direction: column;
		padding-bottom: 80px; /* Space for fixed button */
	}

	.hero-section {
		height: 40vh;
		width: 100%;
		position: relative;
	}

	.image-container {
		width: 100%;
		height: 100%;
		position: relative;
	}

	.image-container img {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	.overlay {
		position: absolute;
		inset: 0;
		background: linear-gradient(to bottom, rgba(0,0,0,0.3), transparent 50%, #000 100%);
	}

	.back-btn {
		position: absolute;
		top: 1.5rem;
		left: 1.5rem;
		width: 40px;
		height: 40px;
		border-radius: 50%;
		background: rgba(0,0,0,0.5);
		border: 1px solid rgba(255,255,255,0.2);
		color: white;
		display: flex;
		align-items: center;
		justify-content: center;
		backdrop-filter: blur(4px);
		cursor: pointer;
		z-index: 10;
	}

	.content-body {
		padding: 0 1.5rem;
		margin-top: -2rem; /* Overlap hero */
		position: relative;
		z-index: 2;
	}

	.header-info h1 {
		font-size: 2rem;
		font-weight: 700;
		margin: 0 0 0.5rem 0;
		line-height: 1.2;
	}

	.location-text {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		color: rgba(255,255,255,0.7);
		margin: 0 0 1.5rem 0;
		font-size: 0.95rem;
	}

	.specs-row {
		display: flex;
		align-items: center;
		justify-content: space-between;
		background: #111;
		padding: 1rem;
		border-radius: 12px;
		border: 1px solid rgba(255,255,255,0.1);
		margin-bottom: 2rem;
	}

	.spec-item {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.25rem;
	}

	.divider {
		width: 1px;
		height: 30px;
		background: rgba(255,255,255,0.1);
	}

	.label {
		font-size: 0.75rem;
		color: rgba(255,255,255,0.5);
		text-transform: uppercase;
		letter-spacing: 0.05em;
	}

	.value {
		font-weight: 600;
		font-size: 0.95rem;
	}

	.bid-stats-card {
		background: linear-gradient(135deg, #222, #111);
		border-radius: 16px;
		padding: 1.5rem;
		border: 1px solid rgba(255,255,255,0.1);
		margin-bottom: 2rem;
	}

	.stat-title {
		font-size: 0.85rem;
		color: rgba(255,255,255,0.6);
		margin-bottom: 1rem;
		text-transform: uppercase;
		font-weight: 600;
	}

	.avg-bid-row {
		display: flex;
		justify-content: space-between;
		align-items: baseline;
		margin-bottom: 0.75rem;
	}

	.avg-label {
		font-size: 1rem;
		font-weight: 500;
	}

	.avg-value {
		font-size: 1.5rem;
		font-weight: 700;
		color: #4ade80; /* Green shade */
	}

	.bid-bar {
		height: 6px;
		background: rgba(255,255,255,0.1);
		border-radius: 3px;
		overflow: hidden;
		margin-bottom: 0.75rem;
	}

	.bar-fill {
		height: 100%;
		background: #4ade80;
	}

	.stat-desc {
		font-size: 0.85rem;
		color: rgba(255,255,255,0.5);
		margin: 0;
	}

	.details-section h3 {
		font-size: 1.25rem;
		margin: 0 0 0.75rem 0;
	}

	.details-section p {
		font-size: 0.95rem;
		line-height: 1.6;
		color: rgba(255,255,255,0.7);
		margin: 0;
	}

	.bottom-action {
		position: fixed;
		bottom: 0;
		left: 0;
		width: 100%;
		padding: 1rem 1.5rem 1.5rem 1.5rem;
		background: linear-gradient(to top, #000 80%, transparent);
		box-sizing: border-box;
		z-index: 100;
	}
</style>
