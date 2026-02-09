<script lang="ts">
	import Button from '$lib/components/Button.svelte';
	import Marqueeck from '@arisbh/marqueeck';
	import { onMount } from 'svelte';

	let messageindex = $state(0);
	let commitindex = $state(0);

	let charIndex = $state(0);
	const totalChars = 41;
	let initialDone = $derived(charIndex >= totalChars);

	let projectString = $state('newproject');
	let commitString = $state('Initial commit');

	let projectShown = $state('');
	let commitShown = $state('');
	let editingField = $state<'project' | 'commit' | null>(null);

	function vis(text: string, start: number) {
		return text.slice(0, Math.max(0, Math.min(text.length, charIndex - start)));
	}

	function hid(text: string, start: number) {
		return text.slice(Math.max(0, Math.min(text.length, charIndex - start)));
	}

	function delay(ms: number): Promise<void> {
		return new Promise((resolve) => setTimeout(resolve, ms));
	}

	async function changeProject(to: string) {
		if (!initialDone || editingField !== null) return;

		const old = projectShown;
		editingField = 'project';
		// Erase current text
		for (let i = old.length; i >= 0; i--) {
			projectShown = old.slice(0, i);
			await delay(10);
		}

		// Type new text
		for (let i = 0; i <= to.length; i++) {
			projectShown = to.slice(0, i);
			await delay(30 + Math.random() * 60);
		}

		projectString = to;
		editingField = null;
	}

	async function changeCommit(to: string) {
		if (!initialDone || editingField !== null) return;
		if (to.length > 20) to = to.slice(0, 19) + '...';

		const old = commitShown;
		editingField = 'commit';
		// Erase current text
		for (let i = old.length; i >= 0; i--) {
			commitShown = old.slice(0, i);
			await delay(10);
		}

		// Type new text
		for (let i = 0; i <= to.length; i++) {
			commitShown = to.slice(0, i);
			await delay(30 + Math.random() * 60);
		}

		commitString = to;
		editingField = null;
	}

	const projects = [
		'simple-todo-list',
		'untraceable-chat',
		'commit-ysws',
		'meditation-guru',
		'linux-kernel',
		'smart-things-connector',
		'custom-firmware',
		'weneta-jelbrek',
		'asahi-gentoo',
		'jarvis'
	];
	const commits = [
		'fixed thermonuclear war',
		'added sleep',
		'fixed zero-day',
		'made app 1% faster',
		'changed database to postgres',
		'added gray mode',
		'I GIVE UP ON CODING',
		'updated readme',
		'refactored 1000 lines',
		'fix',
		'feature',
		'bug',
		'doc',
		'bugfix',
		'fix bufg'
	];
	const onHoverCommits = [
		'Code review pending!',
		'Never do a git reset',
		'Semantic versioning',
		'Pull before you push',
		'Commit early and often',
		"Don't commit secrets",
		'Use branches for features',
		'Rebase and merge',
		'Merge conflicts are fun!'
	];

	onMount(() => {
		let timer: ReturnType<typeof setInterval>;
		projectShown = projectString;
		commitShown = commitString;
		timer = setInterval(() => {
			if (charIndex < totalChars) {
				charIndex++;
			} else {
				clearInterval(timer);
			}
		}, 50);

		let timer1 = setInterval(() => {
			let n = Math.floor(Math.random() * projects.length);
			while (n === messageindex) {
				n = Math.floor(Math.random() * projects.length);
			}
			messageindex = n;
			changeProject(projects[messageindex]);
		}, 5000);
		let timer2: ReturnType<typeof setInterval>;
		setTimeout(() => {
			timer2 = setInterval(() => {
				let n = Math.floor(Math.random() * commits.length);
				while (n === commitindex) {
					n = Math.floor(Math.random() * commits.length);
				}
				commitindex = n;
				changeCommit(commits[commitindex]);
			}, 5000);
		}, 2000);
		return () => {
			clearInterval(timer);
			clearInterval(timer1);
			clearInterval(timer2);
		};
	});

	let currentHoverMessage = $state(0);

	let shopItems = [
		{ name: 'Github Gift card', description: 'Expand your horizons... or at least your codespace limits. A $20 gift card for GitHub', image: '/github.jpg' },
		{ name: 'Hack Club Stickers', description: 'A pack of sum Hack Club stickers to decorate your laptop, water bottle, or whatever you want!', image: '/pile_of_stickers.webp' },
		{ name: 'Commit Hoodie', description: 'A hoodie with the Commit logo on it. Definitely worth wearing out to events. DESIGN NOT FINAL.', image: '/hoodie.png' },
	];
	let shopItems2 = [
		{ name: 'Personal hosting', description: "Every journey needs to start somewhere, but also needs to be sent somewhere.", image: '/serverhosting.png'},
		{ name: 'Random Access Memory', description: "Get that sweet sweet 256MB? of ram that you've always wanted.", image: "/dram.jpg"},
		{ name: "Keyboard Grant", description: 'We shall grant you a keyboard of due power, clickety clacks, and some lighting.', image: "/keyboard.jpg"}
	]

	const faqs = [
		{
			question: 'When will this start?',
			answer: 'Well into the future, hoping that you guys show interest!!!!'
		},
		{
			question: 'How can I track my progress?',
			answer: 'You will be using Hackatime to track time spent coding. More hours = more points, but quality also matters a lot!'
		},
		{
			question: 'Can I BYOP? (bring your own project)',
			answer: 'No, this YSWS is about working on projects collectively as a group of people.'
		},
		{
			question: 'Will the shop be expanded?',
			answer: 'Absolutely! Join the Slack channel to suggest items you want to see in the shop.'
		},
		{
			question: "Can I help out?",
			answer: 'Yes! Join the Slack channel to find out how you can help out, whether that be through coding, design, or just spreading the word.'
		},
		{
			question: "I don't understand. Who do I ask for help?",
			answer: 'Join the Slack channel and ask away! We have a bunch of people there who would be more than happy to help you out with any questions you have, whether it be about the YSWS or just coding in general.'
		}
	];
</script>

<!-- initialDone because we want to have the project and commit messages change around for fun -->
<div
	class="flex h-120 w-full flex-col items-center justify-center gap-5 bg-black/40 text-white backdrop-blur-3xl transition-all"
>
	<div class="flex w-full flex-row items-center justify-center gap-4 font-mono text-3xl">
		<div class="flex flex-1 flex-row justify-end gap-2 truncate text-right text-gray-300">
			<span
				class="opacity-50"
				class:typing={initialDone ? editingField === 'project' : charIndex <= 10}
			>
				{initialDone ? projectShown + '$' : vis(projectString + '$', 0)}
			</span>
			<span class="invisible">{initialDone ? '' : hid(projectString + '$', 0)}</span>
			<span class:typing={!initialDone && charIndex >= 11 && charIndex <= 14}>
				{initialDone ? ' git' : vis(' git', 11)}
			</span>
			<span class="invisible">
				{initialDone ? '' : hid(' git', 11)}
			</span>
		</div>
		<div
			class="group relative flex flex-col font-bold tracking-wider transition-all hover:scale-120 hover:-rotate-3"
			onmouseenter={() => {
				currentHoverMessage = (currentHoverMessage + 1) % onHoverCommits.length;
			}}
			tabindex="-1"
			role="none"
		>
			<div class="flex cursor-pointer flex-row rounded-lg border border-green-300 bg-green-700 p-3">
				<span
					class:typing={!initialDone && charIndex >= 15 && charIndex <= 20}
					class="animate-pulse"
				>
					{initialDone ? 'commit' : vis('commit', 15)}
				</span>
				<span class="invisible">
					{initialDone ? '' : hid('commit', 15)}
				</span>
			</div>
			<span
				class="absolute -bottom-4 -z-10 w-full rounded-b-sm bg-black/60 p-0.5 pt-2 text-right font-sans text-[10px] font-normal tracking-normal text-gray-200 opacity-0 backdrop-blur-3xl transition-all group-hover:opacity-100"
			>
				<span class="">{onHoverCommits[currentHoverMessage]}</span>
			</span>
		</div>
		<div class="flex flex-1 flex-row gap-2 truncate text-gray-300">
			<span class:typing={!initialDone && charIndex >= 21 && charIndex <= 24}>
				{initialDone ? '-am ' : vis('-am ', 21)}
			</span>
			<span class="invisible">
				{initialDone ? '' : hid('-am ', 21)}
			</span>
			<span
				class="opacity-50"
				class:typing={initialDone ? editingField === 'commit' : charIndex >= 25}
			>
				{initialDone ? `"${commitShown}"` : vis(`"${commitString}"`, 25)}
			</span>
			<span class="invisible">{initialDone ? '' : hid(`"${commitString}"`, 25)}</span>
		</div>
	</div>
	<div>Write PRs on HC community projects. Get free stuff.</div>

	<div class="flex flex-row items-center justify-center gap-4">
		<Button
			onclick={() => {
				window.location.href = 'https://atomtables.fillout.com/t/cEgp6ZqE6qus';
			}}>RSVP Today!</Button
		>
		<Button
			onclick={() => {
				window.location.href = 'https://hackclub.enterprise.slack.com/archives/C0AE4S0S2CR';
			}}>Join the Slack!</Button
		>
	</div>
</div>

<div class="-mt-2.5 flex flex-row items-center justify-center">
	<hr class="flex-1" />
	<div class="z-500">
		an in-draft <b>hack club</b> ysws
	</div>
	<hr class="flex-1" />
</div>
<div class="w-full">
	<div class="flex flex-col items-center justify-center gap-4 py-20">
		<div class="text-4xl font-bold">you ship?</div>
		<div class="max-w-2xl p-2 text-center">
			In this YSWS, you commit and make PRs on our public community projects. These community
			projects come from Commit, and are purely worked on by people like you! As you commit and get
			your code merged, you gain more trust in the community based on the quality of your work, and
			can gain higher roles and even the ability to <b>help manage (and start) your own community projects</b>! Every
			completed PR can earn you points in the shop. You can then get items from the shop based on
			the amount of points you have.
		</div>
		<h3>tl;dr:</h3>
		<div class="flex flex-row gap-2 *:rounded-2xl *:bg-black/20 *:backdrop-blur-3xl *:border *:border-green-200/40 *:hover:bg-black/40 *:hover:scale-105 *:transition-all">
			<div
				class="bg-green-/50 flex h-48 w-48 flex-col items-center justify-center rounded-lg p-4 text-center"
			>
				<div class="text-xl font-bold">Issue</div>
				<div>Create an issue with what you want to add or fix.</div>
			</div>
			<div
				class="bg-green-/50 flex h-48 w-48 flex-col items-center justify-center rounded-lg p-4 text-center"
			>
				<div class="text-xl font-bold"><code>git clone</code></div>
				<div>Clone a copy of the repository to your computer.</div>
			</div>
			<div
				class="bg-green-/50 flex h-48 w-48 flex-col items-center justify-center rounded-lg p-4 text-center"
			>
				<div class="text-xl font-bold"><code>git commit</code></div>
				<div>Write code to help fix a bug, add a feature, or improve the project.</div>
			</div>
			<div
				class="bg-green-/50 flex h-48 w-48 flex-col items-center justify-center rounded-lg p-4 text-center"
			>
				<div class="text-xl font-bold"><code>git push</code></div>
				<div>Push your changes to the remote repository and create a pull request.</div>
			</div>
			<div
				class="bg-green-/50 flex h-48 w-48 flex-col items-center justify-center rounded-lg p-4 text-center"
			>
				<div class="text-xl font-bold">Merge</div>
				<div>Get your pull request merged into the main branch.</div>
			</div>
		</div>
	</div>
	<div class="flex max-w-full flex-col items-center justify-center overflow-x-hidden overflow-y-visible py-20">
		<div class="text-4xl font-bold">we ship!</div>
		<div class="max-w-2xl p-2 text-center">cool stuff. not final as of now!!!!</div>
		<div class="w-full bg-slate-800 my-2 overflow-visible">
			<Marqueeck>
				{#each shopItems as item}
					<div class="my-5 flex max-h-36 flex-row border-green-300/20 border-2 hover:bg-green-500/20 group transition-all rounded-2xl overflow-visible">
						<img src={item.image} alt={item.name} class="group-hover:rotate-6 transition-all overflow-visible rounded-lg bg-white p-5 h-36 aspect-auto" />
						<div class="bg-green-/50 flex w-56 flex-col justify-center rounded-lg p-4 text-left">
							<div class="text-xl font-bold">{item.name}</div>
							<div>{item.description}</div>
						</div>
					</div>
				{/each}
			</Marqueeck>
		</div>
		<div class="w-full bg-slate-800 my-2 overflow-visible">
			<Marqueeck speed={40}>
				{#each shopItems2 as item}
					<div class="my-5 flex max-h-36 flex-row border-green-300/20 border-2 hover:bg-green-500/20 group transition-all rounded-2xl overflow-visible">
						<img src={item.image} alt={item.name} class="group-hover:rotate-6 transition-all overflow-visible rounded-lg bg-white p-5 h-36 aspect-auto" />
						<div class="bg-green-/50 flex w-56 flex-col justify-center rounded-lg p-4 text-left">
							<div class="text-xl font-bold">{item.name}</div>
							<div>{item.description}</div>
						</div>
					</div>
				{/each}
			</Marqueeck>
		</div>
	</div>

	<div class="flex flex-col items-center justify-center gap-6 py-20">
		<div class="text-4xl font-bold">faq</div>
		<div class="max-w-2xl text-center text-gray-100">
			quick answers while you wait for your next build to finish.
		</div>
		<div class="grid w-full max-w-5xl grid-cols-1 gap-4 md:grid-cols-2">
			{#each faqs as item}
				<div class="rounded-2xl border border-green-200/40 bg-black/30 p-5 backdrop-blur-2xl">
					<div class="text-lg font-semibold text-white">{item.question}</div>
					<div class="mt-2 text-sm text-gray-200">{item.answer}</div>
				</div>
			{/each}
		</div>
	</div>

</div>
