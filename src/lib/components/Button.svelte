<script>
	export let disabled = false;
</script>

<button class="primary-button" on:click {disabled}>
	<span><slot /></span>
</button>

<style>
	.primary-button {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 0.75rem;
		width: 100%;
		background: #fff; /* Changed to white */
		color: black;
		border: 2px solid black; /* Added black stroke */
		padding: 1.25rem;
		border-radius: 8px; /* Changed border-radius */
		font-size: 0.75rem;
		font-weight: 700;
		font-family: 'Inter', sans-serif;
		text-transform: uppercase; /* Added for full caps */
		cursor: pointer;
		transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
		box-shadow: 0 10px 20px -5px rgba(0, 0, 0, 0.2); /* Adjusted shadow for black background */
	}

	.primary-button:active:not(:disabled) {
		transform: scale(0.96);
		background: #f0f0f0; /* Light gray on active */
	}

	.primary-button:disabled {
		opacity: 0.5;
		cursor: not-allowed;
		background: #ccc;
		border-color: #999;
		color: #666;
		box-shadow: none;
	}

	.primary-button span {
		margin-top: -2px; /* Optical alignment */
	}
</style>
