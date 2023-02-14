<script>
	import { onMount } from 'svelte';
	import { user } from '@fintrack/lib/auth/auth.js';

	let loginRef;

	onMount(() => {
		const google = window.google;

		google.accounts.id.initialize({
			client_id: import.meta.env.VITE_GOOGLE_CLIENT_ID,
			callback: handleCredentialResponse
		});
		google.accounts.id.prompt();
		google.accounts.id.renderButton(loginRef, { size: 'large' });
	});

	function handleCredentialResponse(response) {
		console.log(response);
		$user = response;
	}
</script>

<div class="h-full grid place-items-center">
	<div class="grid justify-items-center gap-4">
		<h1>FinTrack</h1>
		<p>Your personal finance tracker</p>
		<div bind:this={loginRef} />
	</div>
</div>
