<script lang="ts">
	import { onDestroy, onMount, tick } from 'svelte';

	type ScriptItem = {
        prompt: string;
		output: string[];
	};

	const promptPrefix = 'commit@ysws:~$ ';

	const commandScript: ScriptItem[] = [
		{
			prompt: 'curl https://commit.atomtables.dev/issue/create',
			output: ['{', '  "status": "ok",', '  "issue": 42', '}']
		},
		{
			prompt: 'git clone https://commit.atomtables.dev/YOUR_USERNAME/cool-project.git',
			output: [
				"Cloning into 'cool-project'...",
				'remote: Enumerating objects: 82, done.',
				'remote: Counting objects: 100% (82/82), done.',
				'Receiving objects: 100% (82/82), 12.34 MiB | 8.00 MiB/s, done.',
				'Resolving deltas: 100% (18/18), done.'
			]
		},
		{
			prompt: 'git commit -m "add readme"',
			output: [
				'[main 1a2b3c4] add readme',
				' 1 file changed, 1 insertion(+)',
				' create mode 100644 README.md'
			]
		},
		{
			prompt: 'git push origin main',
			output: [
				'Enumerating objects: 8, done.',
				'Counting objects: 100% (8/8), done.',
				'Delta compression using up to 8 threads',
				'Compressing objects: 100% (4/4), done.',
				'Writing objects: 100% (5/5), 1.23 KiB | 1.23 MiB/s, done.',
				'To commit.atomtables.dev:YOUR_USERNAME/cool-project.git',
				'   1a2b3c4..5d6e7f8  main -> main'
			]
		},
		{
			prompt: 'curl https://commit.atomtables.dev/pull_request/create',
			output: ['{', '  "status": "ok",', '  "commits": "1"', '}']
		}
	];

	type Line = {
		kind: 'prompt' | 'output';
		text: string;
	};

	let lines: Line[] = [];
	let running = true;
	let bodyEl: HTMLDivElement | null = null;

	async function scrollToBottom() {
		await tick();
		if (bodyEl) {
			bodyEl.scrollTop = bodyEl.scrollHeight;
		}
	}

	function wait(ms: number) {
		return new Promise<void>((resolve) => setTimeout(resolve, ms/4));
	}

	async function typeLine(text: string, kind: Line['kind'], prefix = '') {
		lines = [...lines, { kind, text: prefix }];
		const index = lines.length - 1;
		let current = '';

		for (const char of text) {
			if (!running) return;
			current += char;
			lines[index].text = prefix + current;
			lines = [...lines];
			await scrollToBottom();
			await wait(kind === 'prompt' ? 28 + Math.random() * 24 : 12 + Math.random() * 18);
		}

		await wait(180);
	}

	async function playScript() {
		while (running) {
			lines = [];

			for (const command of commandScript) {
				await typeLine(command.prompt, 'prompt', promptPrefix);
				for (const output of command.output) {
					await typeLine(output, 'output');
				}
				await wait(260);
			}

			await wait(1200);
		}
	}

	onMount(() => {
		playScript();
	});

	onDestroy(() => {
		running = false;
	});
</script>

<div
	class="w-full min-w-48 max-w-240 overflow-hidden rounded-2xl border border-emerald-200/25 bg-green-900/20 shadow-[0_12px_40px_rgba(0,0,0,0.55)] backdrop-blur-sm"
>
	<div
		class="flex items-center gap-2 border-b border-emerald-200/20 px-4 py-3 text-xs uppercase tracking-[0.14em] text-emerald-100"
	>
		<div class="flex items-center gap-2">
			<span class="h-2.5 w-2.5 rounded-full bg-red-500 shadow-2xl shadow-white" aria-hidden="true"></span>
			<span class="h-2.5 w-2.5 rounded-full bg-amber-500 shadow-2xl shadow-white" aria-hidden="true"></span>
			<span class="h-2.5 w-2.5 rounded-full bg-green-500 shadow-2xl shadow-white" aria-hidden="true"></span>
		</div>
	</div>
	<div
		class="flex h-80 flex-col gap-1 overflow-y-auto overflow-x-hidden bg-black/40 px-4 py-4 font-mono text-sm text-emerald-200/90"
		bind:this={bodyEl}
	>
		{#each lines as line, i}
			<div
				class={`whitespace-pre leading-relaxed ${
					line.kind === 'prompt' ? 'text-emerald-100' : 'text-emerald-200/90'
				}`}
			>
				{line.text}{#if i === lines.length - 1}<span class="ml-1 inline-block h-[1em] w-[0.55em] rounded-sm bg-emerald-200/90 align-middle animate-pulse" aria-hidden="true"></span>{/if}
			</div>
		{/each}
	</div>
</div>
