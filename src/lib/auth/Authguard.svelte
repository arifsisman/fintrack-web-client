<script>
	import { user } from './auth.js';
</script>

{#if $user}
	<slot name="authed" />
{:else}
	<slot name="not_authed" />
{/if}
