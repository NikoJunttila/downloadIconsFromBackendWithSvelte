<script lang="ts">
	import Dropbox from '$lib/Dropbox.svelte';
	import Textbox from '$lib/Textbox.svelte';
	import type { Icons } from '$lib/interfaces';
	import { onMount } from 'svelte';
	import Loader from '$lib/Loader.svelte';
	import { toastStore } from '@skeletonlabs/skeleton';
	import type { ToastSettings } from '@skeletonlabs/skeleton';
	import Snowrain from '$lib/Snowrain.svelte';

	const error: ToastSettings = {
		message: 'Error loading icons. Reload page and try again',
		background: 'variant-filled-error',
		classes: 'text-lg'
	};

	let textboxNames: string[] = [];
	let dropboxNames: string[] = [];
	$: reqNames = textboxNames.concat(dropboxNames);
	let icons: Icons[] = [];
	//localhost:8080
	let server: string = 'http://localhost:8080';
	async function fetchIcons() {
		loading = true;
		try {
			const response = await fetch(`${server}/get/${selected}`);
			const jason = await response.json();
			if (jason) {
				icons = jason;
			} else {
				toastStore.trigger(error);
			}
		} catch (error) {
			console.error('Error fetching icons:', error);
			return error;
		}
		loading = false;
	}
	async function fetchIconSetsNames() {
		loading = true;
		try {
			const response = await fetch(`${server}/iconsets`);
			iconSets = await response.json();
		} catch (error) {
			console.error('Error fetching icons:', error);
			return error;
		}
		loading = false;
	}
	async function doPost() {
		loading = true;
		try {
			const res = await fetch(`${server}/post/centria`, {
				method: 'POST',
				body: JSON.stringify({
					theme: selected,
					icons: reqNames
				}),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			let newIcons = await res.json();
			console.log(newIcons);
			if (newIcons != null) {
				icons = newIcons;
			} else {
				toastStore.trigger(error);
			}
		} catch (error) {
			console.error(error);
		}
		loading = false;
	}
	async function downloadFromBackend() {
		loading = true;
		try {
			const res = await fetch(`${server}/download`, {
				method: 'POST',
				body: JSON.stringify({
					theme: selected,
					icons: reqNames
				}),
				headers: {
					'Content-Type': 'application/json'
				}
			});
			let blob = await res.blob();
			const url = window.URL.createObjectURL(blob);
			const a = document.createElement('a');
			a.style.display = 'none';
			a.href = url;
			a.download = 'icons.zip';
			document.body.appendChild(a);
			a.click();
			window.URL.revokeObjectURL(url);
		} catch (error) {
			console.error(error);
		}
		loading = false;
	}
	async function searchIcons() {
		loading = true;
		try {
			const res = await fetch(`${server}/search`, {
				method: 'POST',
				body: JSON.stringify({
					theme: selected,
					searchTerm: searchText
				}),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			let newIcons = await res.json();
			console.log(newIcons);
			if (newIcons != null) {
				icons = newIcons;
			} else {
				toastStore.trigger(error);
			}
		} catch (error) {
			console.error(error);
		}
		loading = false;
	}
	let iconSets: string[] = ['centria', 'centriaDark', 'oxygen', 'breeze'];
	let selected: string = 'centria';
	let searchText: string = '';

	onMount(() => {
		fetchIcons();
		fetchIconSetsNames();
	});
	let loading: boolean = false;
	import Konami from '$lib/Konami.svelte';
	let konami: boolean = false;
</script>

<Konami bind:konami />
{#if konami}
	<Snowrain />
{/if}
<section class="mx-5">

	<div class="grid place-items-center">
		{#if konami}
		be server loc: <input type="text" bind:value={server} /> <button on:click={() => konami = false}>hide</button> 
		{/if}
		<span
			>select theme: <select class="text-black" bind:value={selected} on:change={fetchIcons}>
				{#each iconSets as set}
					<option value={set}>
						{set}
					</option>
				{/each}
			</select>
		</span>
		<div class="flex">
			<button on:click={doPost}>
				<span />
				<span />
				<span />
				<span /> get icons
			</button>
			<button on:click={downloadFromBackend}
				><span />
				<span />
				<span />
				<span /> Download
			</button>
		</div>
		<div class="flex gap-2 my-3">
			<input
				class="pl-3 rounded drop-shadow-md text-black bg-slate-200"
				type="text"
				bind:value={searchText}
			/>
			<button on:click={searchIcons}
				><span />
				<span />
				<span />
				<span />search</button
			>
		</div>
		<div>
			<div class="text-center">
				<span class="text-lg font-bold">Looking for: </span>
				{#each reqNames as name}
					<span class="mr-2">{name}</span>
				{:else}
					<p>no icon names added yet...</p>
				{/each}
			</div>
			{#if loading}
				<div class="flex justify-center py-3">
					<Loader />
				</div>
			{/if}
			<div class="flex flex-wrap gap-2 icon-container">
				{#each icons as icon}
					<figure>
						<img class="rotate-center icon" src={icon.url} alt={icon.name} title={icon.name} />
						<p class="ml-1">{icon.size}</p>
					</figure>
				{:else}
					<p>loading...</p>
				{/each}
			</div>
		</div>
		<div class="flex">
			<div class="justify-self-end">
				<Textbox bind:cleanedArr={textboxNames} />
			</div>
			<div class="grid items-center p-4"><h2>OR</h2></div>
			<div class="justify-self-start">
				<Dropbox bind:matchingLines={dropboxNames} />
			</div>
		</div>
	</div>
</section>
<div class="absolute top-0 left-0 flex items-center">
	<img
		alt="konami"
		width="170"
		src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e2/Konami_Code.svg/600px-Konami_Code.svg.png"
	/><sub class="-translate-y-1">+ enter</sub>
</div>

<style>
	.icon {
		width: 50px;
		height: 50px;
		margin: 2px;
	}
	div {
		max-width: 100vw;
	}
	.rotate-center:hover {
		-webkit-animation: rotate-center 0.4s ease-in-out both;
		animation: rotate-center 0.4s ease-in-out both;
	}

	@-webkit-keyframes rotate-center {
		0% {
			-webkit-transform: rotate(0);
			transform: rotate(0);
		}
		100% {
			-webkit-transform: rotate(360deg);
			transform: rotate(360deg);
		}
	}
	@keyframes rotate-center {
		0% {
			-webkit-transform: rotate(0);
			transform: rotate(0);
		}
		100% {
			-webkit-transform: rotate(360deg);
			transform: rotate(360deg);
		}
	}
	button {
		position: relative;
		padding: 10px;
		outline: none;
		border: 1px solid #303030;
		background: #20034c;
		color: #ae00ff;
		text-transform: uppercase;
		letter-spacing: 2px;
		font-size: 15px;
		overflow: hidden;
		transition: 0.2s;
		border-radius: 20px;
		cursor: pointer;
		font-weight: bold;
		margin: 2px;
	}

	button:hover {
		box-shadow: 0 0 10px #ae00ff, 0 0 25px #001eff, 0 0 50px #ae00ff;
		transition-delay: 0.6s;
	}

	button span {
		position: absolute;
	}

	button span:nth-child(1) {
		top: 0;
		left: -100%;
		width: 100%;
		height: 2px;
		background: linear-gradient(90deg, transparent, #ae00ff);
	}

	button:hover span:nth-child(1) {
		left: 100%;
		transition: 0.7s;
	}

	button span:nth-child(3) {
		bottom: 0;
		right: -100%;
		width: 100%;
		height: 2px;
		background: linear-gradient(90deg, transparent, #001eff);
	}

	button:hover span:nth-child(3) {
		right: 100%;
		transition: 0.7s;
		transition-delay: 0.35s;
	}

	button span:nth-child(2) {
		top: -100%;
		right: 0;
		width: 2px;
		height: 100%;
		background: linear-gradient(180deg, transparent, #ae00ff);
	}

	button:hover span:nth-child(2) {
		top: 100%;
		transition: 0.7s;
		transition-delay: 0.17s;
	}

	button span:nth-child(4) {
		bottom: -100%;
		left: 0;
		width: 2px;
		height: 100%;
		background: linear-gradient(360deg, transparent, #001eff);
	}

	button:hover span:nth-child(4) {
		bottom: 100%;
		transition: 0.7s;
		transition-delay: 0.52s;
	}

	button:active {
		background: #ae00af;
		background: linear-gradient(to top right, #ae00af, #001eff);
		color: #bfbfbf;
		box-shadow: 0 0 8px #ae00ff, 0 0 8px #001eff, 0 0 8px #ae00ff;
		transition: 0.1s;
	}

	button:active span:nth-child(1) span:nth-child(2) span:nth-child(2) span:nth-child(2) {
		transition: none;
		transition-delay: none;
	}
	select {
		appearance: none;
		border: 0;
		outline: 0;
		font: inherit;
		width: 15em;
		height: 3em;
		padding: 0 4em 0 1em;
		text-align: center;
		text-transform: uppercase;
		background: url(https://upload.wikimedia.org/wikipedia/commons/9/9d/Caret_down_font_awesome_whitevariation.svg)
				no-repeat right 0.8em center / 1.4em,
			linear-gradient(to left, rgba(255, 255, 255, 0.3) 3em, rgba(255, 255, 255, 0.2) 3em);
		border-radius: 0.25em;
		box-shadow: 0 0 1em 0 rgba(0, 0, 0, 0.2);
		cursor: pointer;
		margin-bottom: 5px;
	}
	&::-ms-expand {
		display: none;
	}
</style>
