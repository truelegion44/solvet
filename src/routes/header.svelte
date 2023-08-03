<script>
	import logo from '$lib/logo.svg';
	import dataset from '$lib/stays.json';
	import stringSimilarity from 'string-similarity';
	import Grid from './grid.svelte';
	let none = true;
	$: newData = dataset;

	let Input;
	let searchResults;
	function searchPropertiesByCity(Input) {
		// Convert the user input and property cities to lowercase for case-insensitive matching
		const userInput = Input.toLowerCase();

		// Filter properties based on the closest city name match
		const matches = stringSimilarity.findBestMatch(
			userInput,
			dataset.map((property) => property.city.toLowerCase())
		);
		console.log(matches);

		const closestMatch = matches.bestMatch.target;

		const filteredProperties = dataset.filter((property) => {
			return property.city.toLowerCase() === closestMatch;
		});

		return filteredProperties;
	}

	const removeBackdrop = () => {
		none = !none;
		Filter_data();
	};
	const Filter_data = () => {
		console.log(Input);
		if (Input) {
			searchResults = searchPropertiesByCity(Input);
			console.log(searchResults);
			newData = searchResults;
		} else {
			newData = dataset;
			Input = '';
			searchResults[0].city = null;
		}
	};
</script>

<header class="">
	<!-- BACKDROP -->
	<div class="backdrop bg-[rgba(0,0,0,.1)] absolute  w-full h-[100vh]" class:none>
		<div class=" shadow-lg items-start pt-5 bg-white h-[28rem] absolute  w-full ">
			<div class="flex justify-center flex-col md:flex-row m-auto w-[90%] pb-3">
				<span
					on:click={() => removeBackdrop()}
					class="material-icons material-symbols-outlined cursor-pointer absolute top-0 left-3"
					>close</span
				>
				<input
					type="text"
					class="shadow-lg p-3 md:w-1/3 w-4/5"
					placeholder="Location"
					on:input={() => Filter_data()}
					bind:value={Input}
				/>
				<input type="text" class="shadow-lg p-3 md:w-1/3 w-4/5" placeholder="Guest" />
				<button
					on:click={() => removeBackdrop()}
					class="w-[100px] md:right-64  lg:h-[48px] lg:top-[0px] bottom-[0px] m-3 text-white color-white absolute p-3 bg-[#EB5757] rounded-2xl"
					>Search
				</button>
			</div>
			<div class="  w-4/5 m-auto">
				{#if searchResults}
					{#each searchResults as result}
						<div class="flex items-center m-3">
							<span class="material-symbols-outlined"> location_on </span>
							<p class="">{result.city}:{result.country}</p>
						</div>
					{/each}
				{:else}
					<p>No Recent Searches</p>
				{/if}
			</div>
		</div>
	</div>
	<nav
		class="flex lg:justify-between justify-center  xl:w-[1200px] mb-3 m-auto mt-3 flex-col lg:flex-row "
	>
		<a href="#" class="brand text-center"><img src={logo} alt="" /></a>
		<ul class="m-auto lg:m-0">
			<li class="shadow-lg flex align-middle rounded-lg w-[300px] ">
				{#if searchResults}
					<input
						class="outline-none rounded-lg  w-2/5 p-3"
						type="text"
						on:click={() => removeBackdrop()}
						bind:value={searchResults[0].city}
					/>
				{:else}
					<input
						class="outline-none  rounded-lg  w-2/5 p-3"
						on:click={() => removeBackdrop()}
						type="text"
					/>
				{/if}
				<input
					type="text"
					placeholder="Add Guest"
					class="outline-none w-2/5 border-l-2 p-3 border-l-slate-300 border-r-2 border-r-slate-300"
				/>
				<span
					on:click={() => removeBackdrop()}
					class="material-icons material-symbols-outlined  cursor-pointer w-1/5 search-icon  p-3 text-[#EB5757]"
					>search</span
				>
			</li>
		</ul>
	</nav>
</header>
<Grid {newData} />

<style>
	.none {
		display: none;
	}
</style>
