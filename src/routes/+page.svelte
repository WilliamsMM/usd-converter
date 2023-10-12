<script lang="ts">
	import { PUBLIC_URL_API } from '$env/static/public';
	import axios from 'axios';
	import { onMount } from 'svelte';
	import Icon from '@iconify/svelte';

	let price: string | number;
	let amount_usd: string | number;
	let amount_bs: string | number = '';
	let isReverse = false;

	$: if (amount_bs === 'NaN') {
		amount_bs = '';
	}

	$: if (amount_usd === 'NaN') {
		amount_bs = '';
	}

	function handleInputUsd(e: Event) {
		const target = e.currentTarget as HTMLInputElement;
		target.value = removeLetters(target.value);

		if (!price || !amount_usd) {
			amount_bs = '';
			return;
		}
		const calcToBs = Number(amount_usd) * Number(price);
		amount_bs = convertirCantidad(calcToBs) ?? '';
	}

	function handleInputBs() {
		if (!price || !amount_bs) {
			amount_usd = '';
			return;
		}

		const calcToBs = Number(amount_bs) / Number(price);
		amount_usd = convertirCantidad(calcToBs) ?? '';
	}

	function convertirCantidad(cantidad: number): string {
		const cantidadFormateada = cantidad.toLocaleString('es-ES');
		return cantidadFormateada ?? '0';
	}

	async function getUsdPrice() {
		price = '...';
		const data = await axios.get(PUBLIC_URL_API);
		price = parseFloat(data.data.sources.BCV.quote).toFixed(2);
	}

	function removeLetters(input: string): string {
		return input.replace(/[^0-9]/g, '') ?? '';
	}

	onMount(async () => {
		await getUsdPrice();

		setInterval(async () => {
			await getUsdPrice();
		}, 10000);
	});
</script>

<div class="w-full h-full py-5 px-2">
	<b class="text-white">$1 = {price ?? '...'} Bs</b>
	<div
		class="flex flex-col {isReverse
			? 'flex-col-reverse'
			: ''} bg-white gap-4 mx-auto max-w-lg px-6 py-14 rounded-xl mt-10 shadow-2xl"
	>
		<label class="flex flex-col gap-2" for="amount_usd">
			<b>Amount (USD)</b>
			<div class="flex items-center gap-3">
				<input
					class="border w-full border-gray-300 px-1 py-2 rounded-lg outline-none focus:border-purple-700"
					id="amount_usd"
					type="text"
					placeholder="0.00"
					bind:value={amount_usd}
					on:input={handleInputUsd}
					disabled={isReverse}
				/>
				<strong class="text-2xl text-gray-600">$</strong>
			</div>
		</label>

		<div class="flex justify-center">
			<button
				class="active:scale-95 shadow-2xl rounded-full"
				type="button"
				on:click={() => isReverse = !isReverse}
			>
				<Icon
					icon="akar-icons:arrow-up-down"
					class="text-[45px] bg-purple-500 text-white p-2.5 rounded-full mt-2"
				/>
			</button>
		</div>

		<label class="flex flex-col gap-2" for="amount_usd">
			<b>Amount (BS)</b>
			<div class="flex items-center gap-3">
				<input
					disabled={!isReverse}
					class="border w-full border-gray-300 px-1 py-2 rounded-lg"
					id="amount_usd"
					type="text"
					placeholder="0.00"
					bind:value={amount_bs}
					on:input={handleInputBs}
				/>
				<strong class="text-xl text-gray-600">Bs</strong>
			</div>
		</label>
	</div>
</div>