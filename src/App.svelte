<style>
	.small-main {
		padding-top: 15%;
		padding-left: 30%;
		padding-right: 30%;
	}
	

	.med-main {
		padding-top: 5%;
		padding-left: 20%;
		padding-right: 20%;
	}

	@media all and (max-width: 500px)  {
		.small-main {
			padding-top: 1%;
			padding-left: 2%;
			padding-right: 2%;
		}
	}
</style>

<script>
	import { onMount } from 'svelte';

	import Login from './components/Login.svelte';
	import ActionMenu from './components/ActionMenu.svelte';
	import Header from './components/Header.svelte';
	import RunRoute from './components/RunRoute.svelte';

	let isLoggedIn;
	let useBigClass;

	onMount(async () => {
		reset();
		isLoggedIn = true;
	});

	function handleLogin(event) {
		// TODO: This should handle stuff with roles and permissions later
		isLoggedIn = true;
	}

	function handleLogout(event) {
		isLoggedIn = false;
		reset();
	}

	let activeComponent;

	function handleNavigate(event) {
		console.log('is this happening: ' + event.detail.destination);
		activeComponent = event.detail.destination;
	}

	function reset() {
		activeComponent = 'actionmenu';
		useBigClass = false;
	}
</script>

{#if isLoggedIn}
	<Header on:navigate={handleNavigate} on:logout={handleLogout}/>
{/if}

<main class="{useBigClass ? 'med-main' : 'small-main'}">
	{#if !isLoggedIn}
		<Login on:login={handleLogin}/>
	{:else}
		{#if activeComponent == 'actionmenu'}
			<ActionMenu on:navigate={handleNavigate}/>
		{:else if activeComponent == 'runroute'}
			<RunRoute routeId={1} on:navigate={handleNavigate}/>
		{:else}
			<h3 style="color: red">Well this is awkward.  This should never happen...</h3>
			<h4>Try signing out and restarting your browser.</h4>
		{/if}
	{/if}
</main>