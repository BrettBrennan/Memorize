<script>
	import { GAME_STATE } from './stores';
	import Game from './Components/UI/Game.svelte';
	import Menu from './Components/UI/Menu.svelte';
	import Footer from './Components/UI/Footer.svelte';
	function changeState(e) {
		GAME_STATE.set(e.detail.state);
	}
</script>

<style>
	main {
		text-align: center;
		min-height: 94vh;
	}
	h1 {
		color: #96eca1;
		font-size: 4rem;	
		padding: 50px;
	}
</style>

<main>
	<h1>Memorize</h1>
	{#if $GAME_STATE === 'MAIN_MENU'}
		<Menu on:changeState={changeState} STATE={$GAME_STATE} />
	{:else}
		<Game on:changeState={changeState} STATE={$GAME_STATE} />
	{/if}
</main>

<Footer />