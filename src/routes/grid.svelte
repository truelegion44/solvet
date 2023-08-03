<script>
	export let newData;
	console.log(newData);
</script>

<div class=" m-auto mt-5" id="grid">
	<div class="flex justify-between">
		<h1 class="text-2xl font-bold">Stays In Finland</h1>
		<p>{(newData && newData.length) || 12} stays</p>
	</div>
	<div class="grid grid-cols-1 md:grid-cols-2  lg:grid-cols-3   gap-8 mt-5 " id="grid">
		{#each newData as data}
			<div class="card">
				<img src={data.photo} alt="" class="h-[267px] w-full rounded-3xl" />
				<div class="md:flex justify-between mt-2">
					{#if data.superHost}
						<p
							class="border-2 w-[110px] border-black px-1  font-bold text-sm rounded-2xl capitalize"
						>
							SUPER HOST
						</p>
					{/if}
					<div class="flex text-gray-500">
						<p class="mr-2">{data.type}</p>
						{#if data.beds}
							<p>{data.beds || ''} beds</p>
						{/if}
					</div>
					<div class="flex">
						<span class="material-symbols-outlined mr-2 text-[#EB5757]"> star </span>
						<p>{data.rating}</p>
					</div>
				</div>
				<p class="font-bold">{data.title}</p>
			</div>
		{/each}
	</div>
</div>

<style>
	#grid {
		max-width: 1200px;
		margin: auto;
	}
	@media (max-width: 1200px) {
		#grid {
			max-width: 90%;
		}
	}
</style>
