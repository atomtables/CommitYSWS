<script lang="ts">
	import './layout.css';
	import { fade } from 'svelte/transition';
	import { onMount } from 'svelte';

	let { children } = $props();

	let mounted = $state(false);
	let finishedAnimation = $state(false);
	onMount(() => {
		mounted = true;
		setTimeout(() => {
			finishedAnimation = true;
		}, 500);
	});

</script>

<svelte:head>
	<link rel="icon" href="/favicon.png" />
	<title>Commit - Hack Club YSWS</title>
</svelte:head>

{#if mounted} 
<a href="//hackclub.com">
<img src="/flag-orpheus-left.svg" alt="Hack Club!" class="absolute -left-2 top-8 rotate-12 w-32 hover:scale-120 transition-all z-5000">
</a>

<main class="flex flex-col items-center justify-center h-screen overflow-auto bg-primary" in:fade={{ duration: 500 }}>
	<div class="dark:bg-black/50 w-full h-full overflow-auto">
		{#if finishedAnimation}
			<div in:fade={{ duration: 500 }} class="w-full h-full">
				{@render children()}
			</div>
		{/if}
	</div>
</main>
{/if}
