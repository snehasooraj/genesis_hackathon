<script>
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';
    import { scale, fade } from 'svelte/transition';

    let showCheck = false;

    onMount(() => {
        showCheck = true;
    });

    function handleHome() {
        goto('/buyer/home');
    }
</script>

<div class="success-screen">
    {#if showCheck}
        <div class="circle" in:scale={{ duration: 500, start: 0.5 }}>
            <svg xmlns="http://www.w3.org/2000/svg" width="60" height="60" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"></polyline></svg>
        </div>
        <h1 in:fade={{ delay: 300 }}>Offer Sent!</h1>
        <p in:fade={{ delay: 400 }}>The owner has received your offer.</p>
        
        <button class="home-btn" in:fade={{ delay: 600 }} on:click={handleHome}>Back to Home</button>
    {/if}
</div>

<style>
    .success-screen {
        min-height: 100vh;
        min-height: 100svh;
        background: #000;
        color: white;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        text-align: center;
        padding: 2rem;
    }

    .circle {
        width: 100px;
        height: 100px;
        border-radius: 50%;
        background: #4ade80;
        color: black;
        display: flex;
        align-items: center;
        justify-content: center;
        margin-bottom: 2rem;
        box-shadow: 0 0 30px rgba(74, 222, 128, 0.4);
    }

    h1 {
        margin: 0 0 0.5rem 0;
        font-size: 2rem;
    }

    p {
        color: rgba(255,255,255,0.6);
        margin: 0 0 3rem 0;
    }

    .home-btn {
        background: rgba(255,255,255,0.1);
        border: 1px solid rgba(255,255,255,0.2);
        color: white;
        padding: 1rem 2rem;
        border-radius: 50px;
        font-size: 1rem;
        cursor: pointer;
        transition: background 0.2s;
    }

    .home-btn:hover {
        background: rgba(255,255,255,0.2);
    }
</style>
