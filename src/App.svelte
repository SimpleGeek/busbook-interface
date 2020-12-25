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
	import AddStop from './components/AddStop.svelte';

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
	let callback = null;

	function handleNavigate(event) {
		/*
		 * The general idea here is that the standard destination
		 * from a navigate event can be overridden when a callback
		 * has been set by a previous navigation call.  This is useful,
		 * for example, in the event that a stop is being added from within
		 * the RunRoute component.  The RunRoute component can set a "callback"
		 * to point to itself, so that the AddStop component will be redirected
		 * back to RunRoute once work there is complete.
		 */
		if (callback != null) {
			activeComponent = callback;
		} else {
			activeComponent = event.detail.destination;
		}
		
		if (event.detail.hasOwnProperty('callback')) {
			callback = event.detail.callback;
		} else {
			callback = null;
		}
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
		{:else if activeComponent == 'addstop'}
			<AddStop routeId={1} on:navigate={handleNavigate}/>
		{:else}
			<h3 style="color: red">Well this is awkward.  This should never happen...</h3>
			<h4>Try signing out and restarting your browser.</h4>
		{/if}
	{/if}
</main>