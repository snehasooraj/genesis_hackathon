<script>
	import Button from '$lib/components/Button.svelte';
	import CompactPropertyCard from '$lib/components/CompactPropertyCard.svelte';
	import { goto } from '$app/navigation';
	import { fade, fly } from 'svelte/transition';
	
	// Mock location
	let location = "Hyderabad, Telangana";
	
	// Mock trending properties
	const properties = [
		{
			id: 1,
			price: "1.5 Lakhs/mo",
			image: "https://images.unsplash.com/photo-1512917774080-9991f1c4c750?auto=format&fit=crop&w=500&q=80",
			title: "Modern Villa",
			location: "Jubilee Hills",
			bhk: "4 BHK",
			area: "3500 sqft",
			commute: "15 min",
			rating: 4.8
		},
		{
			id: 2,
			price: "45000 /mo",
			image: "https://images.unsplash.com/photo-1564013799919-ab600027ffc6?auto=format&fit=crop&w=500&q=80",
			title: "City Apartment",
			location: "Gachibowli",
			bhk: "3 BHK",
			area: "1800 sqft",
			commute: "30 min",
			rating: 4.5
		},
		{
			id: 3,
			price: "2.5 Lakhs/mo",
			image: "https://cf.bstatic.com/xdata/images/hotel/max1024x768/466378675.jpg?k=47439be8a91e422a1dbef4f02630d6c86f1266a815d7bd8f21b2c5ce0492bcc1&o=",
			title: "Luxury Estate",
			location: "Banjara Hills",
			bhk: "5 BHK",
			area: "5000 sqft",
			commute: "45 min",
			rating: 4.9
		},
		{
			id: 4,
			price: "30000 /mo",
			image: "https://luxurystays.in/villas/AzulD/BD1.jpg",
			title: "Cozy Condo",
			location: "Madhapur",
			bhk: "2 BHK",
			area: "1200 sqft",
			commute: "10 min",
			rating: 4.2
		},
		{
			id: 5,
			price: "80000 /mo",
			image: "https://images.unsplash.com/photo-1600607687939-ce8a6c25118c?auto=format&fit=crop&w=500&q=80",
			title: "Seaside Home",
			location: "Hitech City",
			bhk: "4 BHK",
			area: "2800 sqft",
			commute: "60 min",
			rating: 4.7
		},
		{
			id: 6,
			price: "15000 /mo",
			image: "https://images.unsplash.com/photo-1448630360428-65456885c650?auto=format&fit=crop&w=500&q=80",
			title: "Studio Loft",
			location: "Kondapur",
			bhk: "1 BHK",
			area: "600 sqft",
			commute: "5 min",
			rating: 4.0
		}
	];

	let searchQuery = "";
	let searchResults = [];

	$: {
		if (searchQuery.trim().length > 0) {
			searchResults = properties.filter(p => 
				p.title.toLowerCase().includes(searchQuery.toLowerCase()) || 
				p.price.toLowerCase().includes(searchQuery.toLowerCase()) ||
				p.bhk.toLowerCase().includes(searchQuery.toLowerCase())
			);
		} else {
			searchResults = [];
		}
	}

	function handleSearchSubmit() {
		if (searchQuery.trim().length > 0) {
			goto(`/property-list?q=${encodeURIComponent(searchQuery)}`);
		}
	}

	function handleResultClick(term) {
		goto(`/property-list?q=${encodeURIComponent(term)}`);
	}


	function handlePropertyClick(property) {
		const params = new URLSearchParams({
			title: property.title,
			price: property.price,
			image: property.image,
			bhk: property.bhk || '3 BHK', // Fallbacks if data missing
			area: property.area || '2000 sqft',
			location: "Hyderabad" // Default location for now
		});
		goto(`/bid?${params.toString()}`);
	}

	function handleMic() {
		// Route to new voice results page
		goto('/voice-results');
	}
</script>

<div class="screen-container">
	<!-- Top Bar -->
	<header class="top-bar">
		<div class="location-chip">
			<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path><circle cx="12" cy="10" r="3"></circle></svg>
			<span>{location}</span>
		</div>
		<div class="avatar">
			<img src="https://ui-avatars.com/api/?name=User&background=333&color=fff" alt="User" />
		</div>
	</header>

	<main>
		<!-- Search Section -->
		<section class="search-section">
			<div class="search-box">
				<form on:submit|preventDefault={handleSearchSubmit}>
					<input 
						type="text" 
						placeholder="Search for properties..." 
						bind:value={searchQuery}
					/>
				</form>
				<svg class="search-icon" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>
				
				{#if searchQuery.trim().length > 0}
					<div class="search-dropdown" transition:fly|local={{ y: 10, duration: 200 }}>
						{#if searchResults.length > 0}
							{#each searchResults as result}
								<button class="search-item" on:click={() => handlePropertyClick(result)}>
									<img src={result.image} alt="" class="item-thumb" />
									<div class="item-info">
										<div class="item-title">{result.title}</div>
										<div class="item-sub">{result.bhk} • {result.price}</div>
									</div>
								</button>
							{/each}
						{:else}
							<div class="no-results">
								No results available
							</div>
						{/if}
						<button class="search-submit-btn" on:click={handleSearchSubmit}>
							See all results for "{searchQuery}"
						</button>
					</div>
				{/if}
			</div>
		</section>

		<!-- Trending Section -->
		<section class="trending-section">
			<h2>Trending Properties Near You</h2>
			
			<div class="scroll-container">
				{#each properties as property, i}
					<CompactPropertyCard 
						{property} 
						index={i} 
						on:click={() => handlePropertyClick(property)} 
					/>
				{/each}
			</div>
		</section>

		<div class="mic-container">
			<button class="mic-button" on:click={handleMic}>
				<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"></path><path d="M19 10v2a7 7 0 0 1-14 0v-2"></path><line x1="12" y1="19" x2="12" y2="23"></line><line x1="8" y1="23" x2="16" y2="23"></line></svg>
				<div class="ripple"></div>
			</button>
		</div>
	</main>

</div>

<style>
	.screen-container {
		min-height: 100vh;
		min-height: 100svh;
		background: #000;
		color: white;
		display: flex;
		flex-direction: column;
		padding: 1.5rem;
		box-sizing: border-box;
		padding-bottom: 2rem;
	}

	.top-bar {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 2rem;
	}

	.location-chip {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		background: rgba(255, 255, 255, 0.1);
		padding: 0.5rem 1rem;
		border-radius: 50px;
		font-size: 0.875rem;
		font-weight: 500;
		color: rgba(255, 255, 255, 0.9);
	}

	.avatar img {
		width: 40px;
		height: 40px;
		border-radius: 50%;
		border: 2px solid rgba(255, 255, 255, 0.2);
	}

	h2 {
		font-size: 1.25rem;
		font-weight: 700;
		margin: 0 0 1rem 0;
		letter-spacing: -0.02em;
	}

	.trending-section {
		margin-bottom: 3rem;
	}

	.scroll-container {
		display: flex;
		gap: 1rem;
		overflow-x: auto;
		padding-bottom: 1rem;
		margin: 0 -1.5rem; /* Break out of container padding */
		padding: 0 1.5rem 1.5rem 1.5rem; /* Add padding back inside scroll area */
		-ms-overflow-style: none;
		scrollbar-width: none;
	}
	
	.scroll-container::-webkit-scrollbar {
		display: none;
	}

	.scroll-container::-webkit-scrollbar {
		display: none;
	}

	main {
		display: flex;
		flex-direction: column;
		flex: 1;
	}

	.search-section {
		width: 100%;
		margin-bottom: 2rem;
		position: relative;
		z-index: 10;
	}

	.mic-container {
		margin-top: 3rem; /* Fixed spacing for "equal space" feel */
		display: flex;
		justify-content: center;
		padding-bottom: 1rem;
	}

	.search-box {
		width: 100%;
		position: relative;
	}

	.search-box input {
		width: 100%;
		background: #111;
		border: 1px solid rgba(255, 255, 255, 0.2);
		padding: 1rem 1rem 1rem 3rem;
		border-radius: 16px;
		color: white;
		font-size: 1rem;
		box-sizing: border-box;
		cursor: text;
		transition: all 0.2s;
	}
	
	.search-box input:focus {
		border-color: #fff;
		outline: none;
	}

	.search-icon {
		position: absolute;
		left: 1rem;
		top: 1rem; /* Fixed top position instead of 50% for stability with dropdown */
		pointer-events: none;
		opacity: 0.5;
	}

	.search-dropdown {
		position: absolute;
		top: 100%;
		left: 0;
		right: 0;
		margin-top: 0.5rem;
		background: #1a1a1a;
		border: 1px solid rgba(255, 255, 255, 0.1);
		border-radius: 12px;
		overflow: hidden;
		z-index: 10;
		box-shadow: 0 10px 30px rgba(0,0,0,0.5);
		max-height: 300px;
		display: flex;
		flex-direction: column;
	}

	/* Scrollable area for items */
	.search-dropdown {
		overflow-y: auto;
	}

	.search-item {
		width: 100%;
		display: flex;
		align-items: center;
		padding: 0.75rem 1rem;
		background: transparent;
		border: none;
		border-bottom: 1px solid rgba(255, 255, 255, 0.05);
		color: white;
		text-align: left;
		cursor: pointer;
		transition: background 0.2s;
	}

	.search-item:hover, .search-item:focus {
		background: rgba(255, 255, 255, 0.05);
		outline: none;
	}

	.item-thumb {
		width: 40px;
		height: 40px;
		border-radius: 6px;
		object-fit: cover;
		margin-right: 1rem;
		background: #333;
	}

	.item-info {
		flex: 1;
	}

	.item-title {
		font-weight: 600;
		font-size: 0.9rem;
	}

	.item-sub {
		font-size: 0.75rem;
		color: rgba(255, 255, 255, 0.5);
	}

	.no-results {
		padding: 2rem;
		text-align: center;
		color: rgba(255, 255, 255, 0.5);
		font-size: 0.9rem;
	}

	.search-submit-btn {
		width: 100%;
		padding: 1rem;
		background: rgba(255, 255, 255, 0.1);
		border: none;
		color: #fff;
		font-weight: 600;
		cursor: pointer;
		font-size: 0.9rem;
		transition: background 0.2s;
	}

	.search-submit-btn:hover {
		background: rgba(255, 255, 255, 0.2);
	}

	.mic-button {
		width: 70px;
		height: 70px;
		border-radius: 50%;
		background: #fff;
		color: black;
		border: none;
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
		position: relative;
		box-shadow: 0 0 20px rgba(255, 255, 255, 0.1);
		transition: transform 0.2s cubic-bezier(0.34, 1.56, 0.64, 1);
	}

	.mic-button:active {
		transform: scale(0.9);
	}
	
	.ripple {
		position: absolute;
		inset: -8px;
		border: 1px solid rgba(255, 255, 255, 0.2);
		border-radius: 50%;
		animation: ripple 2s infinite;
	}
	
	@keyframes ripple {
		0% { transform: scale(1); opacity: 0.5; }
		100% { transform: scale(1.5); opacity: 0; }
	}
</style>
