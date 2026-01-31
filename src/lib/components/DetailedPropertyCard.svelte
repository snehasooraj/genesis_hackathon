<script>
	import { fly } from 'svelte/transition';

	export let property;
	export let index = 0;
</script>

<div class="card" on:click 
     in:fly={{ y: 20, duration: 400, delay: index * 100 }} 
     role="button" 
     tabindex="0"
>
    <div class="card-image">
        <img src={property.image} alt={property.title} />
        <div class="rating">★ {property.rating}</div>
    </div>
    <div class="card-content">
        <div class="left-col">
            <h2>{property.title}</h2>
            {#if property.location}
                <div class="location-row">
                    <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path><circle cx="12" cy="10" r="3"></circle></svg>
                    <span class="location-text">{property.location}</span>
                </div>
            {/if}
            <div class="specs">
                <span>{property.bhk}</span>
                <span class="dot">•</span>
                <span>{property.area}</span>
            </div>
        </div>
        <div class="right-col">
            <div class="price">₹ {property.price}</div>
            <button class="view-btn">View</button>
        </div>
    </div>
</div>

<style>
	.card {
		background: #111;
		border-radius: 16px;
		overflow: hidden;
		border: 1px solid rgba(255, 255, 255, 0.1);
		display: flex;
		flex-direction: column;
		cursor: pointer;
		transition: transform 0.2s;
        box-shadow: 0 4px 10px rgba(0,0,0,0.3);
	}

    .card:active {
        transform: scale(0.98);
    }

	.card-image {
		width: 100%;
		height: 200px;
		position: relative;
	}

	.card-image img {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	.rating {
		position: absolute;
		top: 10px;
		right: 10px;
		background: rgba(0, 0, 0, 0.7);
		color: #ffd700;
		padding: 4px 8px;
		border-radius: 8px;
		font-size: 0.8rem;
		font-weight: 700;
		backdrop-filter: blur(4px);
	}

	.card-content {
		padding: 1rem;
		display: flex;
		justify-content: space-between;
		align-items: flex-end;
	}

	.left-col {
		flex: 1;
	}

	.right-col {
		display: flex;
		flex-direction: column;
		align-items: flex-end;
		gap: 0.5rem;
	}

	h2 {
		font-size: 1.1rem;
		margin: 0 0 0.25rem 0;
		font-weight: 600;
        color: white;
	}

	.location-row {
		display: flex;
		align-items: center;
		gap: 4px;
		margin-bottom: 0.75rem;
	}

	.location-text {
		color: rgba(255, 255, 255, 0.8);
		font-size: 0.9rem;
	}

	.specs {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		font-size: 0.85rem;
		color: rgba(255, 255, 255, 0.6);
	}

	.dot {
		color: rgba(255, 255, 255, 0.3);
	}

	.price {
		font-size: 1.1rem;
		font-weight: 700;
		color: #fff;
	}

	.view-btn {
		background: white;
		color: black;
		border: none;
		padding: 0.5rem 1rem;
		border-radius: 6px;
		font-size: 0.8rem;
		font-weight: 600;
		cursor: pointer;
	}

    @media (min-width: 600px) {
        .card {
            flex-direction: row;
        }
        .card-image {
            width: 250px;
            height: auto;
        }
    }
</style>
