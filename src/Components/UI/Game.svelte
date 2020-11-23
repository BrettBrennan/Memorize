<script>
    import { createEventDispatcher } from 'svelte';
    import Board from './Board.svelte';
    
    const dispatch = createEventDispatcher();

    export let STATE;
    let boardSize = 5;
    let GAME_PLAYING = false;
    function changeState(state) {
        dispatch('changeState', { state });
    }
    function startGame(size) {
        boardSize = size;
        GAME_PLAYING = true;
    }
</script>
<style></style>
<div>
    {#if STATE === 'GAME_PLAY'}
        {#if GAME_PLAYING}
            <Board {boardSize} />
        {:else}
            <h3>Select a board size to begin.</h3>
            <button on:click={() => startGame(6)}>7</button>
            <button on:click={() => startGame(12)}>14</button>
            <button on:click={() => startGame(18)}>21</button>
            <button on:click={() => startGame(24)}>28</button>
        {/if}
    {:else}
        Game Over?  
    {/if}
</div>