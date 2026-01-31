<script>
	import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
	import { fade, fly } from 'svelte/transition';

	import DetailedPropertyCard from '$lib/components/DetailedPropertyCard.svelte';
	let status = 'listening'; // listening, processing, results
	let transcript = "";
	let displayedProperties = [];

	// Mock properties in Kottayam
	const kottayamProperties = [
		{
			id: 101,
			price: "15000 /mo",
			image: "https://images.unsplash.com/photo-1542314831-068cd1dbfeeb?auto=format&fit=crop&w=500&q=80",
			title: "Riverside Cottage",
			location: "Kumarakom, Kottayam",
			bhk: "2 BHK",
			area: "1200 sqft",
			rating: 4.6
		},
		{
			id: 102,
			price: "45000 /mo",
			image: "https://images.unsplash.com/photo-1568605114967-8130f3a36994?auto=format&fit=crop&w=500&q=80",
			title: "Traditional Kerala Home",
			location: "Kanjikuzhy, Kottayam",
			bhk: "4 BHK",
			area: "3000 sqft",
			rating: 4.8
		},
		{
			id: 103,
			price: "25000 /mo",
			image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?auto=format&fit=crop&w=500&q=80",
			title: "Hilltop Villa",
			location: "Pampady, Kottayam",
			bhk: "3 BHK",
			area: "2200 sqft",
			rating: 4.3
		}
	];

	onMount(() => {
		// Simulate voice workflow
		setTimeout(() => {
			status = 'processing';
			transcript = "Properties near Kottayam";
		}, 1500);

		setTimeout(() => {
			status = 'results';
			displayedProperties = kottayamProperties;
		}, 3000);
	});

    function handleBack() {
        goto('/buyer/home');
    }

	function handlePropertyClick(property) {
		const params = new URLSearchParams({
			title: property.title,
			price: property.price,
			image: property.image,
			bhk: property.bhk,
			area: property.area,
			location: property.location
		});
		goto(`/bid?${params.toString()}`);
	}
</script>

<div class="screen-container">
	<header class="header">
        <button class="back-btn" on:click={handleBack} aria-label="Go back">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 12H5"/><path d="M12 19l-7-7 7-7"/></svg>
        </button>
		<h1>Voice Search</h1>
	</header>

	{#if status === 'listening'}
		<div class="center-content" in:fade>
			<div class="mic-animation">
				<div class="mic-icon">
					<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"></path><path d="M19 10v2a7 7 0 0 1-14 0v-2"></path><line x1="12" y1="19" x2="12" y2="23"></line><line x1="8" y1="23" x2="16" y2="23"></line></svg>
				</div>
				<div class="pulse-ring"></div>
				<div class="pulse-ring delay"></div>
			</div>
			<p>Listening...</p>
		</div>
	{:else if status === 'processing'}
		<div class="center-content" in:fade>
			<div class="processing-text">"{transcript}"</div>
			<div class="spinner"></div>
		</div>
	{:else if status === 'results'}
		<div class="results-container" in:fly={{ y: 20, duration: 500 }}>
			<div class="results-header">
				Showing results for "<strong>{transcript}</strong>"
			</div>
			
			<div class="property-list">
				{#each displayedProperties as property, i}
					<DetailedPropertyCard 
						{property} 
						{index} 
						on:click={() => handlePropertyClick(property)} 
					/>
				{/each}
			</div>
		</div>
	{/if}
</div>

<style>
	.screen-container {
		min-height: 100vh;
		min-height: 100svh;
		background: #000;
		color: white;
		padding: 1.5rem;
		box-sizing: border-box;
		display: flex;
		flex-direction: column;
	}

	.header {
        display: flex;
        align-items: center;
        gap: 1rem;
		margin-bottom: 2rem;
	}
    
    .back-btn {
        background: rgba(255, 255, 255, 0.1);
        border: none;
        color: white;
        width: 40px;
        height: 40px;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        transition: background 0.2s;
    }

    .back-btn:hover {
        background: rgba(255, 255, 255, 0.2);
    }

	h1 {
		font-size: 1.5rem;
		font-weight: 700;
		margin: 0;
	}

	.center-content {
		flex: 1;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 2rem;
		padding-bottom: 20vh;
	}

	.mic-animation {
		position: relative;
		width: 80px;
		height: 80px;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.mic-icon {
		position: relative;
		z-index: 2;
		background: white;
		color: black;
		width: 60px;
		height: 60px;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.pulse-ring {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 100%;
		height: 100%;
		border-radius: 50%;
		border: 2px solid rgba(255, 255, 255, 0.5);
		animation: pulse 2s infinite;
	}

	.pulse-ring.delay {
		animation-delay: 0.6s;
	}

	@keyframes pulse {
		0% { width: 60px; height: 60px; opacity: 1; border-width: 2px; }
		100% { width: 140px; height: 140px; opacity: 0; border-width: 0px; }
	}

	.processing-text {
		font-size: 1.5rem;
		font-weight: 600;
		text-align: center;
	}

	.spinner {
		width: 30px;
		height: 30px;
		border: 3px solid rgba(255,255,255,0.1);
		border-top-color: white;
		border-radius: 50%;
		animation: spin 1s linear infinite;
	}

	@keyframes spin {
		to { transform: rotate(360deg); }
	}

	.results-header {
		margin-bottom: 1.5rem;
		color: rgba(255, 255, 255, 0.7);
	}
	
	.results-header strong {
		color: white;
	}

	.property-list {
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
	}

	.card {
		background: #111;
		border-radius: 16px;
		overflow: hidden;
		border: 1px solid rgba(255, 255, 255, 0.1);
		display: flex;
		flex-direction: column;
		cursor: pointer; /* Add pointer cursor */
		transition: transform 0.2s; /* Add transition */
		box-shadow: 0 4px 10px rgba(0,0,0,0.3);
	}

	.card:active {
		transform: scale(0.98); /* Add press effect */
	}
</style>
