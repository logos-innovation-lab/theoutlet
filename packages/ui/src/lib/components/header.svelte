<script lang="ts">
	import UserIcon from '$lib/components/icons/user.svelte'
	import Button from './button.svelte'

	import { goto } from '$app/navigation'
	import { ROUTES } from '$lib/routes'
	import Wallet from './icons/wallet.svelte'
	import Edit from './icons/edit.svelte'
	import UpToTop from './icons/up-to-top.svelte'

	let cls: string | undefined = undefined
	export { cls as class }

	let y: number
	export let loggedin: boolean | undefined = undefined

	function goTop() {
		document.body.scrollIntoView()
	}
</script>

<svelte:window bind:scrollY={y} />

<div class={`root ${y > 0 ? 'scrolled' : ''} ${cls}`}>
	<div class="header">
		<div class="header-title-wrap">
			<Button icon={UpToTop} on:click={goTop} />
			<span class={` ${y > 0 ? 'subtitle' : 'header-title'} ${cls}`}>
				{y > 0 ? 'Public Timeline' : 'The Outlet'}
			</span>
		</div>
		{#if y > 0}
			<Button
				icon={loggedin ? Edit : Wallet}
				variant="primary"
				on:click={() => goto(loggedin ? ROUTES.POST_NEW : ROUTES.PROFILE)}
			/>
		{:else}
			<!-- I BELIEVE THIS IS SUPPOSED BE THE MARKETPLACE'S AVATAR. WHAT IS THE ONCLICK ACTION?? IS THERE ONE? -->
			<Button icon={UserIcon} variant="secondary" on:click={() => goto(ROUTES.PROFILE)} />
		{/if}
	</div>
	{#if y === 0}
		<div class="header-description">
			Milestone 1 shaman pitchfork typewriter single-origin coffee beard flannel, actually
			chillwave.
		</div>
		<div class="subtitle">Public timeline</div>
	{/if}
</div>

<!-- THE ANIMATION IS STILL A BIT WONKY ON THIS PAGE -->
<style lang="scss">
	.root {
		position: sticky;
		top: 0;
		left: 0;
		right: 0;
		padding: var(--spacing-12);
		border-bottom: 1px solid var(--grey-200);
		background-color: rgba(var(--color-body-bg-rgb), 0.93);
		backdrop-filter: blur(3px);

		.hide {
			opacity: 0;
			font-size: 0;
			transition: opacity 0.2s, font-size 0.2s;
		}

		.show {
			opacity: 1;
			transition: opacity 0.2s, font-size 0.2s;
		}

		&.scrolled {
			box-shadow: 0 1px 5px 0 rgba(var(--color-body-text-rgb), 0.25);

			.header {
				padding-bottom: 0;
				transition: padding 0.2s ease-in-out;
			}

			.header-title-wrap {
				margin-left: 0;
				transition: margin 0.2s ease-in-out;
			}

			// .header-description,
			// .subtitle.hide {
			// 	height: 0;
			// 	opacity: 0;
			// 	padding: 0;
			// 	margin: 0;
			// 	font-size: 0;
			// 	transition: height 0.2s, opacity 0.2s, padding 0.2s, margin 0.2s, font-size 0.2s;
			// }
		}

		@media (prefers-color-scheme: dark) {
			border-bottom-color: var(--grey-500);
			&.scrolled {
				box-shadow: 0 1px 5px 0 rgba(var(--color-body-bg-rgb), 0.75);
			}
		}
	}
	.header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 0 0 var(--spacing-24);
		transition: padding 0.2s ease-in-out;
	}

	.header-title {
		font-family: var(--font-body);
		font-weight: 600;
		font-size: 18px;
		font-style: normal;
		text-align: left;
	}

	.header-title-wrap {
		display: flex;
		flex-direction: row;
		align-items: center;
		gap: var(--spacing-12);
		margin-left: -56px;
		transition: margin 0.2s ease-in-out;
	}

	.header-description {
		padding: 0 0 var(--spacing-24);
		opacity: 1;
		// transition: height 0.2s, opacity 0.2s, padding 0.2s, margin 0.2s, font-size 0.2s;
	}

	.subtitle {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-between;
		font-size: 16px;
		font-weight: 600;
		padding: 0;
		overflow: hidden;
		opacity: 1;
		// transition: height 0.5s, opacity 0.5s, padding 0.5s, margin 0.5s, font-size 0.5s;
	}
</style>
