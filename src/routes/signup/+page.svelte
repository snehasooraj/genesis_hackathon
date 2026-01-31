<script>
	import { onMount } from 'svelte';
	import { page } from '$app/stores';
	import { goto } from '$app/navigation';
	import Button from '$lib/components/Button.svelte';

	let role = 'tenant';
	
	// Separate state for each role
	let tenantCreds = { name: '', phone: '', password: '' };
	let ownerCreds = { name: '', phone: '', password: '' };
	
	onMount(() => {
		if (roleParam && (roleParam === 'tenant' || roleParam === 'owner')) {
			role = roleParam;
		}
	});

	$: isTenantValid = tenantCreds.name?.trim() && tenantCreds.phone?.trim() && tenantCreds.password?.trim();
	$: isOwnerValid = ownerCreds.name?.trim() && ownerCreds.phone?.trim() && ownerCreds.password?.trim();

	function handleSubmit() {
		if (role === 'tenant') {
			console.log('Signup as tenant:', tenantCreds);
			goto('/buyer/home');
		} else {
			console.log('Signup as owner:', ownerCreds);
			goto('/seller/dashboard');
		}
	}
</script>

<div class="login-container">
	<div class="card">
		<h1 class="brand">Laddu</h1>
		<p class="subtitle">Create your account to get started.</p>

		<div class="role-selector">
			<button 
				class:selected={role === 'tenant'} 
				on:click={() => role = 'tenant'}
			>
				Tenant
			</button>
			<button 
				class:selected={role === 'owner'} 
				on:click={() => role = 'owner'}
			>
				Owner
			</button>
		</div>

		{#if role === 'tenant'}
			<form on:submit|preventDefault={handleSubmit}>
				<div class="form-group">
					<label for="tenant-name">Full Name <span class="required">*</span></label>
					<input 
						type="text" 
						id="tenant-name" 
						placeholder="Enter your full name" 
						bind:value={tenantCreds.name} 
						required 
					/>
				</div>

				<div class="form-group">
					<label for="tenant-phone">Phone Number <span class="required">*</span></label>
					<input 
						type="tel" 
						id="tenant-phone" 
						placeholder="Enter tenant phone number" 
						bind:value={tenantCreds.phone} 
						required 
					/>
				</div>

				<div class="form-group">
					<label for="tenant-password">Password <span class="required">*</span></label>
					<input 
						type="password" 
						id="tenant-password" 
						placeholder="Create a password" 
						bind:value={tenantCreds.password} 
						required 
					/>
				</div>

				<div class="actions">
					<Button disabled={!isTenantValid}>Sign Up as Tenant</Button>
				</div>
			</form>
		{:else}
			<form on:submit|preventDefault={handleSubmit}>
				<div class="form-group">
					<label for="owner-name">Full Name <span class="required">*</span></label>
					<input 
						type="text" 
						id="owner-name" 
						placeholder="Enter your full name" 
						bind:value={ownerCreds.name} 
						required 
					/>
				</div>

				<div class="form-group">
					<label for="owner-phone">Phone Number <span class="required">*</span></label>
					<input 
						type="tel" 
						id="owner-phone" 
						placeholder="Enter owner phone number" 
						bind:value={ownerCreds.phone} 
						required 
					/>
				</div>

				<div class="form-group">
					<label for="owner-password">Password <span class="required">*</span></label>
					<input 
						type="password" 
						id="owner-password" 
						placeholder="Create a password" 
						bind:value={ownerCreds.password} 
						required 
					/>
				</div>

				<div class="actions">
					<Button disabled={!isOwnerValid}>Sign Up as Owner</Button>
				</div>
			</form>
		{/if}

		<div class="switch-auth">
			<p>Already have an account? <a href="/login" class="link">Login</a></p>
		</div>
	</div>
</div>

<style>
	.login-container {
		min-height: 100vh;
		min-height: 100svh;
		display: flex;
		align-items: center;
		justify-content: center;
		background: #000;
		color: white;
		padding: 1.5rem;
	}

	.card {
		background: #111;
		border: 1px solid rgba(255, 255, 255, 0.1);
		border-radius: 20px;
		padding: 2rem;
		width: 100%;
		max-width: 400px;
		box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4);
	}

	.brand {
		font-size: 2.5rem;
		font-weight: 800;
		margin: 0;
		text-align: center;
		letter-spacing: -0.04em;
		background: linear-gradient(to right, #fff, #aaa);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
	}

	.subtitle {
		text-align: center;
		color: rgba(255, 255, 255, 0.6);
		margin-bottom: 2rem;
		font-size: 0.9rem;
	}

	.role-selector {
		display: flex;
		background: #222;
		border-radius: 10px;
		padding: 4px;
		margin-bottom: 1.5rem;
	}

	.role-selector button {
		flex: 1;
		background: transparent;
		border: none;
		color: #888;
		padding: 0.75rem;
		border-radius: 8px;
		font-weight: 600;
		cursor: pointer;
		transition: all 0.2s ease;
	}

	.role-selector button.selected {
		background: #fff;
		color: #000;
		box-shadow: 0 2px 8px rgba(0,0,0,0.2);
	}

	.form-group {
		margin-bottom: 1.25rem;
	}

	label {
		display: block;
		margin-bottom: 0.5rem;
		font-size: 0.85rem;
		font-weight: 500;
		color: rgba(255, 255, 255, 0.8);
	}

	input {
		width: 100%;
		background: #1a1a1a;
		border: 1px solid rgba(255, 255, 255, 0.1);
		color: white;
		padding: 0.875rem 1rem;
		border-radius: 8px;
		font-size: 1rem;
		outline: none;
		transition: border-color 0.2s;
		box-sizing: border-box;
	}

	input:focus {
		border-color: #fff;
	}

	input::placeholder {
		color: rgba(255, 255, 255, 0.3);
	}
	
	.actions {
		margin-top: 2rem;
	}

	.switch-auth {
		margin-top: 1.5rem;
		text-align: center;
		font-size: 0.9rem;
		color: rgba(255, 255, 255, 0.6);
	}

	.link {
		color: #fff;
		text-decoration: underline;
		cursor: pointer;
	}
	
	.link:hover {
		color: #ccc;
	}

	.required {
		color: #ff4d4d;
		margin-left: 2px;
	}
</style>
