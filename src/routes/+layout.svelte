<script>
	import '@skeletonlabs/skeleton/styles/all.css';
	import '@fintrack/theme.postcss';
	import '@fintrack/app.postcss';
	import Navigation from '@fintrack/lib/navigation/navigation.svelte'; // or import Navigation from '$lib/navigation/navigation.svelte'
	import { AppShell, AppBar } from '@skeletonlabs/skeleton';
	import { Drawer, drawerStore } from '@skeletonlabs/skeleton';
	import { page } from '$app/stores';
	import { onMount } from 'svelte';

	let loginRef;

	onMount(() => {
		const google = window.google;

		google.accounts.id.initialize({
			client_id: import.meta.env.VITE_GOOGLE_CLIENT_ID,
			callback: handleCredentialResponse
		});
		google.accounts.id.prompt();
		google.accounts.id.renderButton(loginRef, {
			size: 'large'
		});
	});

	function drawerOpen() {
		drawerStore.open({});
	}

	function handleCredentialResponse(response) {
		console.log(response);
	}

	$: classesSidebar = $page.url.pathname === '/' ? '' : '';
</script>

<!-- App Shell -->
<AppShell slotSidebarLeft="bg-surface-500/5 w-0 lg:w-64" {classesSidebar}>
	<Drawer>
		<h2 class="p-4">Navigation</h2>
		<hr />
		<Navigation />
	</Drawer>

	<svelte:fragment slot="header">
		<!-- App Bar -->
		<AppBar>
			<svelte:fragment slot="lead">
				<div class="flex items-center">
					<button class="lg:hidden btn btn-sm mr-4" on:click={drawerOpen}>
						<span>
							<svg viewBox="0 0 100 80" class="fill-token w-4 h-4">
								<rect width="100" height="20" />
								<rect y="30" width="100" height="20" />
								<rect y="60" width="100" height="20" />
							</svg>
						</span>
					</button>
					<strong class="text-xl">FinTrack</strong>
				</div>
			</svelte:fragment>

			<svelte:fragment slot="trail">
				<div bind:this={loginRef} />
			</svelte:fragment>
		</AppBar>
	</svelte:fragment>

	<svelte:fragment slot="sidebarLeft">
		<Navigation />
	</svelte:fragment>

	<!-- Page Route Content -->
	<slot />
</AppShell>
