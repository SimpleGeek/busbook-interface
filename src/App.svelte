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
		useBigClass = false;
		isLoggedIn = true;
	});

	function handleLogin(event) {
		// TODO: This should handle stuff with roles and permissions later
		isLoggedIn = true;
	}
</script>

{#if isLoggedIn}
	<Header/>
{/if}

<main class="{useBigClass ? 'med-main' : 'small-main'}">
	{#if !isLoggedIn}
		<Login on:login={handleLogin}/>
	{:else}
		<!--<ActionMenu/>-->
		<RunRoute routeId={1}/>
	{/if}
</main>