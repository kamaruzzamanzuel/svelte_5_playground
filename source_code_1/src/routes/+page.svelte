<script lang="ts">
	import Header from './Header.svelte';
	let age = 10;

	let statedName = $state('Scott');
	let status: 'OPEN' | 'CLOSED' = $state('OPEN');

	let fisrtName = $state('Scott');
	let lastName = $state('Smith');

	let derivedName = $derived(`${fisrtName} ${lastName}`);

	const toggleStatus = () => {
		status = status === 'OPEN' ? 'CLOSED' : 'OPEN';
	};

	const onclick = () => {
		status = status === 'OPEN' ? 'CLOSED' : 'OPEN';
	};

	// #region form state
	let formState = $state({
		name: '',
		birthdate: '',
		step: 0,
		error: ''
	});
	// #endregion
</script>

<main>
	<Header name="Scott" {age} {statedName} {derivedName} />

	<input type="text" bind:value={statedName} />
	<!-- <button on:click={toggleStatus}>{status}</button> -->
	<button onclick={toggleStatus}>{status}</button>
	<button {onclick}>{status}</button>

	<!-- #region derived state -->
	<label aria-label="First Name" for="firstName">First Name</label>
	<input type="text" bind:value={fisrtName} />

	<label aria-label="Last Name" for="lastName">Last Name</label>
	<input type="text" bind:value={lastName} />

	<p>{derivedName}</p>

	<!-- #endregion -->

	<!-- #region form -->
	<h1>Form</h1>
	<p>Step : {formState.step}</p>

	{#if formState.error}
		<p class="error">{formState.error}</p>
	{/if}

	{#if formState.step === 0}
		<div>
			<label for="name">Name</label>
			<input type="text" bind:value={formState.name} />
		</div>

		<button
			onclick={() => {
				if (formState.name) {
					formState.step += 1;
					formState.error = '';
				} else {
					formState.error = 'Name is required';
				}
			}}>Next</button
		>

		{:else if formState.step === 1}
		
		<div>
			<label for="birthdate">Birthdate</label>
			<input type="date" bind:value={formState.birthdate} />
		</div>

		<button
			onclick={() => {
				if (formState.birthdate) {
					formState.step += 1;
					formState.error = '';
				} else {
					formState.error = 'Birthdate is required';
				}
			}}>Next</button
		>
	{/if}
</main>
