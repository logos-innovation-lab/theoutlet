<script lang="ts">
	import Button from '$lib/components/button.svelte'
	import ArrowRight from '$lib/components/icons/arrow-right.svelte'
	import Close from '$lib/components/icons/close.svelte'
	import CodeSigningService from '$lib/components/icons/code-signing-service.svelte'
	import GroupSecurity from '$lib/components/icons/group-security.svelte'
	import Renew from '$lib/components/icons/renew.svelte'
	import UpToTop from '$lib/components/icons/up-to-top.svelte'
	import UserAdmin from '$lib/components/icons/user-admin.svelte'
	import User from '$lib/components/icons/user.svelte'
	import Wallet from '$lib/components/icons/wallet.svelte'
	import Edit from '$lib/components/icons/edit.svelte'
</script>

<h2>Buttons</h2>
<Button label="primary" variant="primary" />
<Button label="secondary" variant="secondary" />
<Button label="primary disabled" variant="primary" disabled />
<Button label="secondary disabled" variant="secondary" disabled />
<Button label="primary icon" variant="primary" icon={UpToTop} />
<Button label="secondary icon" variant="secondary" icon={UpToTop} />
<Button label="primary icon disabled" variant="primary" icon={UpToTop} disabled />
<Button label="secondary icon disabled" variant="secondary" icon={UpToTop} disabled />
<Button variant="primary" icon={UpToTop} />
<Button variant="secondary" icon={UpToTop} />
<Button variant="primary" icon={UpToTop} disabled />
<Button variant="secondary" icon={UpToTop} disabled />

<h2>Icons</h2>

<UpToTop />
<User />
<Wallet />
<GroupSecurity />
<ArrowRight />
<Close />
<CodeSigningService />
<Renew />
<UserAdmin />
<Edit />
