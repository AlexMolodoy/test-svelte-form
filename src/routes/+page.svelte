<script lang="ts">
	let firstName = $state<string>('');
	let lastName = $state<string>('');
	let country = $state<string>('');
	let birthDate = $state<string>('');
	let showMaxTransfer = $state<boolean>(false);
	let countries: Record<string, string> = { USA: '$', Georgia: '₾', UK: '£' };
	let value = $state<string>('');
	let disableForm = $state<boolean>(false);

	function handleSubmit(event: SubmitEvent) {
		event.preventDefault();
		disableForm = !disableForm;

		const formData = {
			firstName,
			lastName,
			country,
			birthDate,
			showMaxTransfer
		};

		console.log('Submitted Data:', formData);
		alert('Editing success!!');
	}

	function formatCurrency(value: string): string {
		const numericValue = value.replace(/[^0-9]/g, '');

		const mainPart = numericValue.slice(0, -2);
		const decimalPart = numericValue.slice(-2);

		const formattedMainPart = mainPart.replace(/\B(?=(\d{3})+(?!\d))/g, ',');

		return `${formattedMainPart}${numericValue.length ? '.' : ''}${decimalPart} ${numericValue.length ? countries[country] || '$' : ''}`;
	}

	function handleInput(event: Event) {
		const target = event.target as HTMLInputElement;
		const inputValue = target.value;
		value = formatCurrency(inputValue);
	}
</script>

<div class="flex items-center justify-center min-h-screen">
	<div
		class="max-w-[400px] md:max-w-[600px] w-full p-5 bg-[#f5f5f5] rounded-lg shadow-lg font-sans"
	>
		<h1 class="text-2xl font-bold mb-6 text-center">Editing User Details</h1>

		<form onsubmit={handleSubmit}>
			<div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
				<div>
					<label class="mb-2 font-semibold text-gray-800" for="firstName">First Name</label>
					<input
						disabled={disableForm}
						type="text"
						id="firstName"
						bind:value={firstName}
						placeholder="Enter your first name"
						class="w-full p-3 border border-gray-300 rounded-lg"
						required
					/>
				</div>

				<div>
					<label class="mb-2 font-semibold text-gray-800" for="lastName">Last Name</label>
					<input
						disabled={disableForm}
						type="text"
						id="lastName"
						bind:value={lastName}
						placeholder="Enter your last name"
						class="w-full p-3 border border-gray-300 rounded-lg"
						required
					/>
				</div>
			</div>

			<div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
				<div>
					<label class="mb-2 font-semibold text-gray-800" for="birthDate">Date of Birth</label>
					<input
						disabled={disableForm}
						type="date"
						id="birthDate"
						bind:value={birthDate}
						class="w-full p-3 border border-gray-300 rounded-lg"
						required
					/>
				</div>

				<div>
					<label class="mb-2 font-semibold text-gray-800" for="country">Country</label>
					<select
						disabled={disableForm}
						id="country"
						bind:value={country}
						class="w-full p-3 border border-gray-300 rounded-lg"
						required
					>
						<option value="" disabled selected>Choose your country</option>
						{#each Object.keys(countries) as country}
							<option value={country}>{country}</option>
						{/each}
					</select>
				</div>
			</div>

			<div class="mb-4">
				<label class="flex items-center space-x-2">
					<input
						disabled={disableForm}
						type="checkbox"
						bind:checked={showMaxTransfer}
						class="h-5 w-5 border border-gray-300 rounded-md"
					/>
					<span class="font-semibold text-gray-800">Internal transfers</span>
				</label>
			</div>

			{#if showMaxTransfer}
				<div class="mb-4 h-[78px]">
					<label class="mb-2 font-semibold text-gray-800" for="transferAmount"
						>Max Transfer Amount</label
					>
					<input
						disabled={disableForm}
						type="text"
						id="transferAmount"
						bind:value
						oninput={handleInput}
						placeholder="0.00 {countries[country] || ''}"
						class="w-full p-3 border border-gray-300 rounded-lg"
					/>
				</div>
			{:else}
				<div class="mb-4 h-[78px]"></div>
			{/if}

			<button
				disabled={disableForm}
				type="submit"
				class="w-full bg-blue-500 hover:bg-blue-700 text-white font-semibold py-3 rounded-lg transition-all"
			>
				Save
			</button>
		</form>
	</div>
</div>

<style>
	input[type='text'],
	input[type='date'],
	select {
		width: 100%;
		padding: 10px;
		border: 1px solid #ccc;
		color: black;
		border-radius: 8px;
		font-size: 1rem;
	}

	input[type='text']:disabled,
	input[type='date']:disabled,
	select:disabled {
		background-color: gray;
	}
</style>
