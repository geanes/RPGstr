<script lang="ts">
	import {
		currentAbilities,
		currentSaves,
		currentAbilityModifiers,
		currentSaveTotals,
		modify
	} from '$lib/stores/storeCharacter';
</script>

<section class="card variant-glass-surface min-w-max" class:variant-ringed-error={$modify}>
	<h2 class="pb-0 p-4 text-slate-50/80">Saving Throws</h2>
	<div class="pt-1 p-4 space-y-2">
		<div
			class="grid grid-cols-8 grid-rows-2 gap-y-0 pb-0 mb-0 gap-2 align-text-bottom text-xs text-slate-50/50 tracking-tighter font-light"
		>
			<span class="col-start-5 row-start-1">Ability</span>
			<span class="col-start-6 row-start-1">Magic</span>
			<span class="col-start-7 row-start-1">Misc</span>
			<span class="col-start-8 row-start-1">Temp</span>
			<span class="col-start-3 row-start-2">Total</span>
			<span class="col-start-4 row-start-1">Base</span>
			<span class="col-start-4 row-start-2">Save</span>
			<span class="col-start-5 row-start-2">Modifier</span>
			<span class="col-start-6 row-start-2">Modifier</span>
			<span class="col-start-7 row-start-2">Modifier</span>
			<span class="col-start-8 row-start-2">Modifier</span>
		</div>
		<!-- Fortitude -->
		<div class="grid grid-cols-8 gap-2">
			<button
				id="fort"
				title="Fortitude"
				class="button col-span-2 bg-surface-900/60 p-2 text-center rounded-sm"
				class:variant-outline-warning={$currentSaves.fortTemp}
				on:click={() => ($currentSaves.fortTemp = !$currentSaves.fortTemp)}
			>
				Fortitude
			</button>
			<div
				id="fortTotal"
				title="Fortitude Total"
				class="bg-slate-500/40 p-2 text-center rounded-full"
			>
				<p>{$currentSaveTotals.fortTotal}</p>
			</div>
			<div id="fortBase" title="Base Save" class="bg-slate-500/40 p-2 text-center rounded-sm">
				<p>{$currentSaves.fortBase}</p>
			</div>
			<div
				id="fortAbilMod"
				title="Constitution Modifier"
				class="bg-slate-500/40 p-2 text-center rounded-sm"
				class:variant-outline-warning={$currentAbilities.con.temp}
			>
				<p>{$currentAbilityModifiers.activeConMod}</p>
			</div>
			<input
				id="fortMagicMod"
				title="Magic Modifier"
				class="input variant-form-material text-center arrow-hide"
				type="number"
				min="0"
				max="20"
				bind:value={$currentSaves.fortMagicMod}
			/>
			<input
				id="fortMiscMod"
				class="input variant-form-material text-center arrow-hide"
				title="Miscellaneous Modifier"
				type="number"
				min="-20"
				max="20"
				bind:value={$currentSaves.fortMiscMod}
			/>
			<input
				id="fortTempMod"
				title="Temporary Modifier"
				class="input variant-form-material text-center arrow-hide"
				type="number"
				min="0"
				max="20"
				bind:value={$currentSaves.fortTempMod}
				disabled={!$currentSaves.fortTemp}
			/>
		</div>
		<!-- Reflex DEX -->
		<div class="grid grid-cols-8 gap-2">
			<button
				id="reflex"
				title="Reflex"
				class="button col-span-2 bg-surface-900/60 p-2 text-center rounded-sm"
				class:variant-outline-warning={$currentSaves.reflexTemp}
				on:click={() => ($currentSaves.reflexTemp = !$currentSaves.reflexTemp)}
			>
				Reflex
			</button>
			<div
				id="reflexTotal"
				title="Reflex Total"
				class="bg-slate-500/40 p-2 text-center rounded-full"
			>
				<p>{$currentSaveTotals.reflexTotal}</p>
			</div>
			<div id="reflexBase" title="Base Save" class="bg-slate-500/40 p-2 text-center rounded-sm">
				<p>{$currentSaves.reflexBase}</p>
			</div>
			<div
				id="reflexAbilMod"
				title="Dexterity Modifier"
				class="bg-slate-500/40 p-2 text-center rounded-sm"
				class:variant-outline-warning={$currentAbilities.dex.temp}
			>
				<p>{$currentAbilityModifiers.activeDexMod}</p>
			</div>
			<input
				id="reflexMagicMod"
				title="Magic Modifier"
				class="input variant-form-material text-center arrow-hide"
				type="number"
				min="0"
				max="20"
				bind:value={$currentSaves.reflexMagicMod}
			/>
			<input
				id="reflexMiscMod"
				class="input variant-form-material text-center arrow-hide"
				title="Miscellaneous Modifier"
				type="number"
				min="-20"
				max="20"
				bind:value={$currentSaves.reflexMiscMod}
			/>
			<input
				id="reflexTempMod"
				title="Temporary Modifier"
				class="input variant-form-material text-center arrow-hide"
				type="number"
				min="0"
				max="20"
				bind:value={$currentSaves.reflexTempMod}
				disabled={!$currentSaves.reflexTemp}
			/>
		</div>
		<!-- Will WIS -->
		<div class="grid grid-cols-8 gap-2">
			<button
				id="will"
				title="Will"
				class="button col-span-2 bg-surface-900/60 p-2 text-center rounded-sm"
				class:variant-outline-warning={$currentSaves.willTemp}
				on:click={() => ($currentSaves.willTemp = !$currentSaves.willTemp)}
			>
				Will
			</button>
			<div id="willTotal" title="Will Total" class="bg-slate-500/40 p-2 text-center rounded-full">
				<p>{$currentSaveTotals.willTotal}</p>
			</div>
			<div id="willBase" title="Base Save" class="bg-slate-500/40 p-2 text-center rounded-sm">
				<p>{$currentSaves.willBase}</p>
			</div>
			<div
				id="willAbilMod"
				title="Dexterity Modifier"
				class="bg-slate-500/40 p-2 text-center rounded-sm"
				class:variant-outline-warning={$currentAbilities.wis.temp}
			>
				<p>{$currentAbilityModifiers.activeWisMod}</p>
			</div>
			<input
				id="willMagicMod"
				title="Magic Modifier"
				class="input variant-form-material text-center arrow-hide"
				type="number"
				min="0"
				max="20"
				bind:value={$currentSaves.willMagicMod}
			/>
			<input
				id="willMiscMod"
				class="input variant-form-material text-center arrow-hide"
				title="Miscellaneous Modifier"
				type="number"
				min="-20"
				max="20"
				bind:value={$currentSaves.willMiscMod}
			/>
			<input
				id="willTempMod"
				title="Temporary Modifier"
				class="input variant-form-material text-center arrow-hide"
				type="number"
				min="0"
				max="20"
				bind:value={$currentSaves.willTempMod}
				disabled={!$currentSaves.willTemp}
			/>
		</div>
	</div>
</section>
