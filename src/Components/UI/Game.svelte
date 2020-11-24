<script>
    import Board from './Board.svelte';
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    export let STATE;
    let boardSize = 6;
    let GAME_PLAYING = false;
    let won = false;
    let finalScore = 0;
    function changeState(state) {
        dispatch('changeState', { state });
    }
    function startGame(size) {
        boardSize = size;
        GAME_PLAYING = true;
    }
    function gameOver(e) {
        changeState('GAME_OVER');
        finalScore = e.detail.score;
        won = e.detail.won;
        GAME_PLAYING = false;
    }
</script>
<style></style>
<div>
    {#if STATE === 'GAME_PLAY'}
        {#if GAME_PLAYING}
            <Board {boardSize} on:gameOver={gameOver} />
        {:else}
            <h3>Select a board size to begin.</h3>
            <button on:click={() => startGame(6)}>6</button>
            <button on:click={() => startGame(12)}>12</button>
            <button on:click={() => startGame(18)}>18</button>
            <button on:click={() => startGame(24)}>24</button>
        {/if}
    {:else if STATE === 'GAME_OVER'}
        {#if won}
            <h2>You Won! Your final score was {finalScore}</h2>
        {:else}
            <h2>You Lost! Your final score was {finalScore}</h2>
        {/if}
    {:else}
        TODO: Add state here.
    {/if}
</div>