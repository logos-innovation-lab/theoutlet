<script lang="ts">
	import type { IconProps } from '$lib/types'

	type $$Props = IconProps

	let cls: string | undefined = undefined
	export { cls as class }
	export let size = 20
</script>

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" width={size} height={size} class={cls}>
	<polygon points="16,14 6,24 7.4,25.4 16,16.8 24.6,25.4 26,24 " />
	<rect x="4" y="8" width="24" height="2" />
</svg>
