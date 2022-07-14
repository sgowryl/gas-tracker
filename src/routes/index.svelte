<script>
	const GAS_PRICE_API = 'https://web.zapper.fi/v1/gas-prices?network=ethereum&eip1559=true';
	const PRICE_FETCH_API =
		'https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false';

	const fetchGasPrice = (async () => {
		const response = await fetch(GAS_PRICE_API);
		return await response.json();
	})();

	const fetchPrice = (async () => {
		const response = await fetch(PRICE_FETCH_API);
		return await response.json();
	})();
</script>

<div class="relative flex items-center justify-center h-screen mb-12 overflow-hidden">
	<div class="relative z-30 w-auto min-w-full min-h-full max-w-none">
		<div class="my-40 mx-40">
			{#await fetchGasPrice}
				<p>...waiting</p>
			{:then data}
				<div
					class="text-center text-5xl py-10 bg-clip-text text-transparent bg-gradient-to-r from-pink-500 to-red-500 font-semibold"
				>
					🔥 Gas price
				</div>
				<div class="grid grid-cols-3 text-xl text-gray-300 gap-5">
					<div
						class="border rounded-lg border-zinc-800 text-center py-10 bg-zinc-900 bg-opacity-40 hover:bg-opacity-80 ease-in duration-200"
					>
						<div class="text-5xl">🐢</div>
						<div class="text-lg text-gray-500 py-3 ">standard</div>
						<div class="text-5xl font-semibold text-gray-200">
							{data.standard.baseFeePerGas}
						</div>
					</div>
					<div
						class="border rounded-lg border-zinc-800 text-center py-10 bg-zinc-900 bg-opacity-40 hover:bg-opacity-80 ease-in duration-200"
					>
						<div class="text-5xl">🐇</div>
						<div class="text-lg text-gray-500 py-3">fast</div>
						<div class="text-5xl font-semibold text-gray-200">
							{data.fast.baseFeePerGas}
						</div>
					</div>
					<div
						class="border rounded-lg border-zinc-800 text-center py-10 bg-zinc-900 bg-opacity-40 hover:bg-opacity-80 ease-in duration-200"
					>
						<div class="text-5xl">🐆</div>
						<div class="text-lg text-gray-500 py-3">instant</div>
						<div class="text-5xl font-semibold text-gray-200">
							{data.instant.baseFeePerGas}
						</div>
					</div>
				</div>
			{:catch error}
				<p>An error occurred!</p>
			{/await}
		</div>

		<div class="text-gray-300 flex overflow-x-auto bg-zinc-900 bg-opacity-80 p-1">
			{#await fetchPrice}
				<p>...waiting</p>
			{:then data}
				{#each data as token}
					<div class="flex mx-4 my-auto">
						<img src={token.image} class="w-5 h-5" alt="" />
						<div class="flex">
							<span class="whitespace-nowrap text-center text-slate-300 pl-1">
								{token.name}
							</span>
							<span class="text-gray-100 font-medium px-1">
								{token.current_price}
							</span>
							<span
								class={token.price_change_percentage_24h > 0
									? ' text-emerald-700'
									: 'text-rose-600'}
							>
								{token.price_change_percentage_24h.toFixed(2)}%
							</span>
						</div>
					</div>
				{/each}
			{:catch error}
				<p>An error occurred!</p>
			{/await}
		</div>
	</div>

	<video
		autoplay
		loop
		muted
		class="absolute z-10 w-auto min-w-full min-h-full max-w-none blur opacity-70"
	>
		<source src="fire.mp4" type="video/mp4" />
		Your browser does not support the video tag.
	</video>
</div>
