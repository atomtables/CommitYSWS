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
		<img
			src="/flag-orpheus-left.svg"
			alt="Hack Club!"
			class="absolute top-8 -left-2 z-5000 w-32 rotate-12 transition-all hover:scale-120"
		/>
	</a>

	<main
		class="bg-primary flex h-screen flex-col items-center justify-center overflow-auto"
		in:fade={{ duration: 500 }}
	>
		{#if finishedAnimation}
			<div in:fade={{ duration: 500 }} class="h-full w-full overflow-auto dark:bg-black/50">
				<div class="h-full w-full">
					{@render children()}
					<footer class="bg-neutral-800/50 backdrop-blur-3xl">
						<div class="flex flex-col p-4 text-sm px-10">
							<img src='/hc.svg' alt='Hack Club' class='mr-5 w-16 py-2' />
							<div class="">A Hack Club initiative</div>
							<div class="">Commit: a Hack Club YSWS</div>
							<div class="">Made with  by <b>atomtables</b> and <b>abominablepug</b></div>
						</div>
					</footer>
				</div>
			</div>
		{/if}
	</main>
{/if}
