<script lang="ts">
	import { browser } from '$app/environment'
	import Button from '$lib/components/button.svelte'
	import Logout from '$lib/components/icons/logout.svelte'
	import Undo from '$lib/components/icons/undo.svelte'
	import Wallet from '$lib/components/icons/wallet.svelte'
	import WalletInfo from '$lib/components/wallet-info.svelte'
	import { formatAddress } from '$lib/utils'
	import { connectWallet, canConnectWallet, createIdentity } from '$lib/services'
	import { profile } from '$lib/stores/profile'

	let error: Error | undefined = undefined
	let hasWallet = browser && canConnectWallet()

	const handleConnect = async () => {
		try {
			$profile.signer = await connectWallet()

			const defaultIdentity = 'anonymous'

			const identity = await createIdentity($profile.signer, defaultIdentity)

			$profile.identities = { ...$profile.identities, [defaultIdentity]: identity }
		} catch (err) {
			error = err as Error
		}
	}

	$: console.log($profile.identities)
</script>

<div class="header">
	<div>
		<Button icon={Undo} on:click={() => history.back()} />
		<h1>Account</h1>
	</div>
</div>
<div class="content">
	{#if $profile.signer === undefined}
		<div class="wallet-icon-wrapper">
			<Wallet size={192} />
		</div>
		<div class="pad">
			<Button
				variant="primary"
				icon={Wallet}
				label="Connect wallet to post"
				on:click={handleConnect}
				disabled={!hasWallet}
			/>
			<span class="connect-info">
				{#if hasWallet}
					Connect a wallet to access or create your account.
				{:else}
					Please install a web3 wallet to access or create your account.
				{/if}
			</span>
			{#if error !== undefined}
				<span>Failed to connect {error.message}</span>
			{/if}
		</div>
	{:else}
		<!-- WHY IS THIS AN INPUT? DOES IT HAVE TO BE? -->
		<div class="wallet-info-wrapper">
			<WalletInfo title="Connected wallet">
				{#await $profile.signer.getAddress()}
					loading...
				{:then address}
					{formatAddress(address)}
				{:catch error}
					{error.message}
				{/await}
			</WalletInfo>
		</div>
		<div class="pad">
			<Button
				variant="primary"
				icon={Logout}
				label="Logout"
				on:click={() => ($profile.signer = undefined)}
				disabled={!$profile.signer}
			/>
		</div>
	{/if}

	{#each Object.entries($profile.identities) as [name, identity]}
		<div>{name}</div>
		<div>commitment: {identity.getCommitment().toString(16)}</div>
		<div>nullifier: {identity.getNullifier().toString(16)}</div>
		<div>trapdoor: {identity.getTrapdoor().toString(16)}</div>
	{/each}
</div>

<style lang="scss">
	.wallet-icon-wrapper :global(svg) {
		fill: var(--grey-100);
	}

	.header {
		padding: var(--spacing-12) var(--spacing-12) 0;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;

		> div:first-child {
			display: flex;
			flex-direction: row;
			align-items: center;
			gap: var(--spacing-12);

			h1 {
				font-weight: 600;
			}
		}
	}
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;

		.pad {
			padding: var(--spacing-12);
			display: flex;
			flex-direction: column;
			align-items: center;
		}

		> div:first-child {
			margin-top: var(--spacing-48);
			margin-bottom: var(--spacing-24);
		}

		.wallet-info-wrapper {
			background-color: var(--grey-100);
			width: 100%;
		}

		.connect-info {
			margin-top: var(--spacing-12);
			text-align: center;
		}
	}
</style>
