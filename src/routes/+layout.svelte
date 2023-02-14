<script>
	import '@skeletonlabs/skeleton/styles/all.css';
	import '@fintrack/theme.postcss';
	import '@fintrack/app.postcss';
	import Navigation from '@fintrack/lib/navigation/navigation.svelte'; // or import Navigation from '$lib/navigation/navigation.svelte'
	import Login from '@fintrack/routes/login/login.svelte';

	import { AppShell, AppBar } from '@skeletonlabs/skeleton';
	import { Drawer, drawerStore } from '@skeletonlabs/skeleton';
	import { user } from '@fintrack/lib/auth/auth.js';

	// import { page } from '$app/stores';
	// import { onMount } from 'svelte';

	// onMount(() => {
	// 	if ($user === undefined && $page.url.pathname !== '/') {
	// 		page.set('/');
	// 	}
	// });

	function drawerOpen() {
		drawerStore.open({});
	}
</script>

{#if $user}
	<AppShell slotSidebarLeft="bg-surface-500/5 w-0 lg:w-64">
		<Drawer>
			<h2 class="p-4">Navigation</h2>
			<hr />
			<Navigation />
		</Drawer>

		<svelte:fragment slot="header">
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
					<a href="/#/login" on:click={() => ($user = undefined)}>Logout</a>
				</svelte:fragment>
			</AppBar>
		</svelte:fragment>

		<svelte:fragment slot="sidebarLeft">
			<Navigation />
		</svelte:fragment>

		<slot />
	</AppShell>
{:else}
	<Login />
{/if}
