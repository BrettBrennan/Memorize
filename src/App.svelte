<script>
	import { writable } from 'svelte/store';
	import { GAME_STATE } from './stores';
	import Game from './Components/UI/Game.svelte';
	import Menu from './Components/UI/Menu.svelte';
	function changeState(state) {
		GAME_STATE.set(state);
	}
</script>

<style>
	main {
		text-align: center;
	}
</style>

<main>
	<h1>Memorize</h1>
	{#if $GAME_STATE === 'MAIN_MENU'}
		<Menu />
	{:else}
		<Game />
	{/if}

</main>