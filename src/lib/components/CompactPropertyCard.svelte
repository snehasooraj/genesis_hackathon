<script>
	import { fly } from 'svelte/transition';

	export let property;
	export let index = 0;
</script>

<button class="property-card" on:click 
	in:fly={{ x: 50, duration: 500, delay: index * 100 }}
>
	<div class="image-wrapper">
		<img src={property.image} alt={property.title} />
		<div class="rating-badge">
			<span>★</span> {property.rating}
		</div>
	</div>
	<div class="card-details">
		<div class="price">
			<span class="rupee">₹</span> {property.price}
		</div>
		<div class="info-row">
			<span>{property.bhk}</span> • <span>{property.area}</span>
		</div>
		<div class="commute">
			<svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"></circle><polyline points="12 6 12 12 16 14"></polyline></svg>
			{property.commute} commute
		</div>
	</div>
</button>

<style>
	.property-card {
		flex: 0 0 200px; /* Wider card for details */
		height: 280px; /* Taller card */
		background: #1a1a1a;
		border-radius: 16px;
		overflow: hidden;
		border: 1px solid rgba(255, 255, 255, 0.1);
		padding: 0;
		cursor: pointer;
		transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1), box-shadow 0.3s;
		position: relative;
		display: flex;
		flex-direction: column;
		text-align: left; /* important for details */
		box-shadow: 0 4px 10px rgba(0,0,0,0.3);
	}

	.property-card:hover {
		transform: scale(1.02) translateY(-5px);
		z-index: 2;
		box-shadow: 0 10px 20px rgba(0,0,0,0.5);
		border-color: rgba(255, 255, 255, 0.5);
	}

	.image-wrapper {
		width: 100%;
		height: 60%; /* Adjusted height */
		position: relative;
	}

	.image-wrapper img {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	.rating-badge {
		position: absolute;
		top: 10px;
		right: 10px;
		background: rgba(0, 0, 0, 0.6);
		color: #ffd700;
		padding: 4px 8px;
		border-radius: 12px;
		font-size: 0.75rem;
		font-weight: 700;
		backdrop-filter: blur(4px);
		display: flex;
		align-items: center;
		gap: 2px;
	}

	.card-details {
		padding: 0.75rem;
		display: flex;
		flex-direction: column;
		gap: 0.25rem;
		flex: 1;
		justify-content: space-between;
	}

	.price {
		font-size: 1.125rem;
		font-weight: 700;
		color: white;
		display: flex;
		align-items: center;
		gap: 4px;
	}
	
	.rupee {
		font-weight: 400;
	}

	.info-row {
		font-size: 0.8rem;
		color: rgba(255, 255, 255, 0.7);
		font-weight: 500;
	}

	.commute {
		font-size: 0.75rem;
		color: rgba(255, 255, 255, 0.5);
		display: flex;
		align-items: center;
		gap: 4px;
		margin-top: auto;
	}
</style>
