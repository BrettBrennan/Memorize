<script>
    import Board from './Board.svelte';
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    export let STATE;

    let boardSize = 12;
    let GAME_PLAYING = false;
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
<style>
     h3 {
        width: 50%;
        color: #e5e5e5;
        margin: auto;
    }
    ul {
        list-style-type: none;
        margin: 0;
        padding: 0;
    }
    button {
        margin: 25px;
        width: 300px;
        border: 0;
		background-image: linear-gradient(120deg, #d4fc79 0%, #96e6a1 100%);
        padding: 5px;
        color: #222;
        font-size: 2rem;
        cursor: pointer;       
    }
</style>
<div>
    {#if STATE === 'GAME_PLAY'}
        {#if GAME_PLAYING}
            <Board {boardSize} on:gameOver={gameOver} />
        {:else}
            <h3>Select a board size to begin.</h3>
            <ul>
                <li><button on:click={() => startGame(12)}>12 Cards</button></li>
                <li><button on:click={() => startGame(16)}>16 Cards</button></li>
                <li><button on:click={() => startGame(20)}>20 Cards</button></li>
                <li><button on:click={() => startGame(24)}>24 Cards</button></li>
                <li><button on:click={() => startGame(36)}>36 Cards</button></li>
                <li><button on:click={() => startGame(48)}>48 Cards</button></li>
            </ul>
        {/if}
    {:else}
        TODO: Add state here.
    {/if}
</div>