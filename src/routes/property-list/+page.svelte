<script>
	import { page } from '$app/stores';
	import { fade, fly } from 'svelte/transition';
	import { onMount } from 'svelte';
    import { goto } from '$app/navigation'; // Import goto
	import DetailedPropertyCard from '$lib/components/DetailedPropertyCard.svelte';

	let query = '';
	let source = '';
	let loading = true;

	// Mock property database (matching what we had in home + more)
	const allProperties = [
		{
			id: 1,
			price: "1.5 Lakhs/mo",
			image: "https://images.unsplash.com/photo-1512917774080-9991f1c4c750?auto=format&fit=crop&w=500&q=80",
			title: "Modern Villa",
			location: "Jubilee Hills, Hyderabad",
			bhk: "4 BHK",
			area: "3500 sqft",
			rating: 4.8
		},
		{
			id: 2,
			price: "45000 /mo",
			image: "https://images.unsplash.com/photo-1564013799919-ab600027ffc6?auto=format&fit=crop&w=500&q=80",
			title: "City Apartment",
			location: "Gachibowli, Hyderabad",
			bhk: "3 BHK",
			area: "1800 sqft",
			rating: 4.5
		},
		{
			id: 3,
			price: "2.5 Lakhs/mo",
			image: "https://cf.bstatic.com/xdata/images/hotel/max1024x768/466378675.jpg?k=47439be8a91e422a1dbef4f02630d6c86f1266a815d7bd8f21b2c5ce0492bcc1&o=",
			title: "Luxury Estate",
			location: "Banjara Hills, Hyderabad",
			bhk: "5 BHK",
			area: "5000 sqft",
			rating: 4.9
		},
		{
			id: 4,
			price: "30000 /mo",
			image: "https://luxurystays.in/villas/AzulD/BD1.jpg",
			title: "Cozy Condo",
			location: "Madhapur, Hyderabad",
			bhk: "2 BHK",
			area: "1200 sqft",
			rating: 4.2
		},
		{
			id: 5,
			price: "80000 /mo",
			image: "https://images.unsplash.com/photo-1600607687939-ce8a6c25118c?auto=format&fit=crop&w=500&q=80",
			title: "Seaside Home",
			location: "Vizag, Andhra Pradesh",
			bhk: "4 BHK",
			area: "2800 sqft",
			rating: 4.7
		},
		{
			id: 6,
			price: "15000 /mo",
			image: "https://images.unsplash.com/photo-1448630360428-65456885c650?auto=format&fit=crop&w=500&q=80",
			title: "Studio Loft",
			location: "Kondapur, Hyderabad",
			bhk: "1 BHK",
			area: "600 sqft",
			rating: 4.0
		}
	];

	let displayedProperties = [];

	onMount(() => {
		query = $page.url.searchParams.get('q') || '';
		source = $page.url.searchParams.get('source') || '';

		// Simulate loading for better UX
		setTimeout(() => {
			if (query) {
				const lowerQ = query.toLowerCase();
				displayedProperties = allProperties.filter(p => 
					p.title.toLowerCase().includes(lowerQ) || 
					p.location.toLowerCase().includes(lowerQ) ||
					p.bhk.toLowerCase().includes(lowerQ)
				);
			} else if (source === 'mic') {
				// For mic usage example, show all or maybe random, 
				// effectively saying "Here's what we found listening..."
				displayedProperties = allProperties;
				query = "Voice Search Results";
			} else {
				displayedProperties = allProperties;
				query = "All Properties";
			}
			loading = false;
		}, 600);
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

<div class="list-container">
	<header class="header">
        <button class="back-btn" on:click={handleBack} aria-label="Go back">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 12H5"/><path d="M12 19l-7-7 7-7"/></svg>
        </button>
		<h1>
			{#if loading}
				Searching...
			{:else}
				{query}
			{/if}
		</h1>
	</header>

	{#if loading}
		<div class="loader-container">
			<div class="spinner"></div>
		</div>
	{:else}
		<div class="property-list">
			{#each displayedProperties as property, i}
				<DetailedPropertyCard 
					{property} 
					{index} 
					on:click={() => handlePropertyClick(property)} 
				/>
			{:else}
				<div class="empty-state">
					<p>No properties found matching "{query}"</p>
				</div>
			{/each}
		</div>
	{/if}
</div>

<style>
	.list-container {
		min-height: 100vh;
		min-height: 100svh;
		background: #000;
		color: white;
		padding: 1.5rem;
		box-sizing: border-box;
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

	.property-list {
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
	}



	.loader-container {
		display: flex;
		justify-content: center;
		padding: 4rem 0;
	}

	.spinner {
		width: 30px;
		height: 30px;
		border: 3px solid rgba(255,255,255,0.1);
		border-top-color: white;
		border-radius: 50%;
		animation: spin 1s linear infinite;
	}

	.empty-state {
		text-align: center;
		padding: 4rem 1rem;
		color: rgba(255, 255, 255, 0.5);
	}

	@keyframes spin {
		to { transform: rotate(360deg); }
	}


</style>
