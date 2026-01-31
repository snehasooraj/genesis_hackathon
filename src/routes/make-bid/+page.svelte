<script>
	import { page } from '$app/stores';
	import { goto } from '$app/navigation';
	import { fade, fly } from 'svelte/transition';

	let amount = "";
	
	// Get context
	$: title = $page.url.searchParams.get('title') || 'Property';

	function handleNumber(num) {
        if (amount.length > 10) return;
        if (num === '.' && amount.includes('.')) return;
		amount += num;
	}

	function handleBackspace() {
		amount = amount.slice(0, -1);
	}

	function handleSubmit() {
		if (!amount) return;
		goto('/offer-success');
	}

    function formatAmount(val) {
        if (!val) return "0";
        // Simple comma formatting for India
        let x = val.split('.');
        let lastThree = x[0].substring(x[0].length-3);
        let otherNumbers = x[0].substring(0,x[0].length-3);
        if(otherNumbers != '')
            lastThree = ',' + lastThree;
        let res = otherNumbers.replace(/\B(?=(\d{2})+(?!\d))/g, ",") + lastThree;
        if (x.length > 1) res += "." + x[1];
        return res;
    }
</script>

<div class="bid-input-screen">
    <div class="top-section">
        <h2>Enter Offer Amount</h2>
        <p>for {title}</p>
        
        <div class="amount-display">
            <span class="currency">₹</span>
            <span class="value">{formatAmount(amount)}</span>
             <span class="cursor">|</span>
        </div>
    </div>

    <div class="keypad">
        {#each [1, 2, 3, 4, 5, 6, 7, 8, 9] as num}
            <button class="key" on:click={() => handleNumber(num)}>{num}</button>
        {/each}
        <button class="key" on:click={() => handleNumber('.')}>.</button>
        <button class="key" on:click={() => handleNumber(0)}>0</button>
        <button class="key backspace" on:click={handleBackspace}>
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 4H8l-7 8 7 8h13a2 2 0 0 0 2-2V6a2 2 0 0 0-2-2z"></path><line x1="18" y1="9" x2="12" y2="15"></line><line x1="12" y1="9" x2="18" y2="15"></line></svg>
        </button>
    </div>

    <button class="submit-fab" on:click={handleSubmit} disabled={!amount} class:active={amount.length > 0}>
        <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"></polyline></svg>
    </button>
</div>

<style>
    .bid-input-screen {
        min-height: 100vh;
        min-height: 100svh;
        background: #000;
        color: white;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
    }

    .top-section {
        flex: 1;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        padding: 2rem;
    }

    h2 {
        font-size: 1.2rem;
        font-weight: 500;
        margin: 0;
        color: rgba(255,255,255,0.7);
    }

    p {
        margin: 0.5rem 0 0 0;
        color: rgba(255,255,255,0.5);
    }

    .amount-display {
        margin-top: 3rem;
        font-size: 3rem;
        font-weight: 700;
        display: flex;
        align-items: center;
        gap: 0.5rem;
    }
    
    .cursor {
        animation: blink 1s infinite;
        font-weight: 100;
        color: #4ade80;
    }
    @keyframes blink { 50% { opacity: 0; } }

    .keypad {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        background: #111;
        padding-bottom: 2rem;
    }

    .key {
        padding: 1.5rem 0;
        background: transparent;
        border: none;
        color: white;
        font-size: 1.5rem;
        font-weight: 500;
        cursor: pointer;
        transition: background 0.2s;
    }

    .key:active {
        background: rgba(255,255,255,0.1);
    }

    .submit-fab {
        position: fixed;
        bottom: 2rem;
        right: 2rem;
        width: 70px;
        height: 70px;
        border-radius: 50%;
        background: #333;
        color: rgba(255,255,255,0.2);
        border: none;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
        pointer-events: none;
    }

    .submit-fab.active {
        background: #4ade80; /* Green */
        color: black;
        transform: scale(1.1);
        pointer-events: auto;
        cursor: pointer;
        box-shadow: 0 10px 30px rgba(74, 222, 128, 0.4);
    }
</style>
