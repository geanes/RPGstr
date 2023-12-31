<script lang="ts">
	import { toastStore } from '@skeletonlabs/skeleton';
	import type { ToastSettings } from '@skeletonlabs/skeleton';
	import {
		AppShell,
		AppBar,
		AppRail,
		AppRailTile,
		AppRailAnchor,
		Avatar,
		ProgressBar
	} from '@skeletonlabs/skeleton';

	import { loadCharacter } from '$lib/utils/characterUtils';
	import { triggerPlayerDrawer } from '$lib/utils/singletonDrawer';
	import EditCharacter from './components/EditCharacter.svelte';
	import CharAvatar from './components/CharAvatar.svelte';
	import CharAttributes from './components/CharAttributes.svelte';
	import {
		currentMetadata,
		currentState,
		currentAttributes,
		shortCharacterDescription
	} from '$lib/stores/storeCharacter';
	import { currentUser } from '$lib/stores/currentUser';
	import ndk from '$lib/stores/ndk';
	import { stringifiedCharacter } from '$lib/stores/storeCharacter';
	import { generateCharacter } from '$lib/utils/eventUtils';

	export let data;
	const { levels, races, alignments, classes } = data;

	const toastResetCharacter: ToastSettings = {
		message: 'Are you sure you want to reset this character?',
		autohide: false,
		background: 'variant-filled-warning',
		action: {
			label: 'Reset',
			response: () => loadCharacter({})
		}
	};

	let currentTile: number = 1;

	function testPlayer() {
		if ($currentUser === undefined) return console.log('No user logged in');
		const result = generateCharacter({
			ndk: $ndk,
			kind: 31974,
			user: $currentUser,
			name: $currentAttributes.name,
			desc: $shortCharacterDescription,
			game: $currentMetadata.campaign,
			content: $stringifiedCharacter,
			system: 'DnD 3.5e',
			uid: $currentMetadata.uid
		});
		return console.log(result);
	}
</script>

<section class="overflow-hidden">
	<AppShell>
		<!-- APP RAIL -->
		<svelte:fragment slot="sidebarLeft">
			<div class="h-screen">
				<AppRail background="bg-surface-800/80">
					<svelte:fragment slot="lead">
						<AppRailAnchor href="/">
							<iconify-icon icon="ri:home-line" class="text-slate-100/80" width="24" />
						</AppRailAnchor>
					</svelte:fragment>
					<AppRailTile bind:group={currentTile} name="tile-1" value={0} title="tile-1">
						<svelte:fragment slot="lead">
							<iconify-icon icon="ph:play-bold" class="text-slate-100/80" width="24" />
						</svelte:fragment>
						<!-- <span>Play</span> -->
					</AppRailTile>
					<AppRailTile bind:group={currentTile} name="tile-2" value={1} title="tile-2">
						<svelte:fragment slot="lead">
							<iconify-icon icon="bx:edit" class="text-slate-100/70" width="24" />
						</svelte:fragment>
						<!-- <span>Edit</span> -->
					</AppRailTile>
					<svelte:fragment slot="trail">
						<div class="flex flex-col justify-center items-stretch text-center">
							<button
								class="btn-icon btn-icon-xl p-0 w-full"
								title="Settings"
								on:click={triggerPlayerDrawer}
							>
								{#if $currentUser}
									<Avatar
										src={$currentUser?.image}
										alt="profilepic"
										width="w-14"
										rounded="rounded-full"
										cursor="cursor-pointer"
										border="border hover:border-2 border-slate-100/70"
										shadow="shadow-2xl hover:shadow-xl"
									/>
								{:else}
									<iconify-icon
										icon="material-symbols:settings-outline"
										class="text-slate-100/80 text-xl"
									/>
								{/if}
							</button>
						</div>
					</svelte:fragment>
				</AppRail>
			</div>
		</svelte:fragment>

		<!-- CHARACTER HEADER -->
		<svelte:fragment slot="pageHeader">
			<AppBar
				gridColumns="grid-cols-3"
				slotDefault="place-self-center"
				slotTrail="place-content-end"
				padding="p-1"
				background="variant-glass-surface/0"
				shadow="shadow-xs"
			>
				<svelte:fragment slot="lead">
					<div class="flex-row">
						<span class="pl-2 text-slate-50/50 align-middle">
							<button
								class="btn-icon btn-icon-sm variant-outline-primary"
								title="Reset Character"
								on:click={() => toastStore.trigger(toastResetCharacter)}
							>
								<iconify-icon icon="fluent-mdl2:reset" class="text-slate-100/80" />
							</button>
						</span>
						<span class="align-center ml-8 text-xl font-extralight text-surface-100/60">
							{$currentMetadata.campaign}
						</span>
					</div>
				</svelte:fragment>
				<svelte:fragment slot="headline">
					<div class="p-0 pr-4 ml-20 mr-32">
						<ProgressBar
							label="XP to next level"
							meter="bg-primary-500/50"
							value={$currentAttributes.xp}
							min={levels[$currentAttributes.level - 1].min}
							max={levels[$currentAttributes.level - 1].max}
						/>
					</div>
				</svelte:fragment>
				<svelte:fragment slot="trail">
					<div class="p-0 ml-20 mr-32">
						<label class="space-x-0">
							<input
								type="number"
								class="input arrow-hide variant-ringed text-end text-orange-400/70 w-24 border-0 font-bold"
								bind:value={$currentState.money}
							/>
							<span class="inline-block text-orange-400/70 p-[8px] pl-0 rounded-full font-bold"
								>GP</span
							>
						</label>
					</div>
				</svelte:fragment>
			</AppBar>
			<span>
				<CharAvatar />
			</span>
			<div class="mx-auto p-4 grid grid-cols-10 gap-2">
				<div class="col-span-10">
					<CharAttributes {levels} {races} {classes} {alignments} condensed={currentTile !== 1} />
				</div>
			</div>
		</svelte:fragment>

		{#if currentTile === 0}
			<!-- EDIT TILE -->
			<button class="btn" on:click={testPlayer}>Log to console</button>
		{:else if currentTile === 1}
			<!-- PLAY TILE -->
			<EditCharacter {data} />
		{/if}
	</AppShell>
</section>
