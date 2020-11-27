<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
    export let STATE;

    function changeState(state) {
        dispatch('changeState', { state });
    }

</script>
<style>
    h1, h3 {
        padding: 10px;
        word-break: keep-all;
        color: #efefef;
    }
    h3 {
        width: 50%;
        color: #e5e5e5;
        margin: auto;
    }
    button {
        margin: 50px;
        width: 300px;
        border: 0;
		background-image: linear-gradient(120deg, #d4fc79 0%, #96e6a1 100%);
        padding: 5px;
        color: #222;
        font-size: 2rem;
        cursor: pointer;
    }
    button:hover {
        opacity: 0.8;
    }
</style>
<div>
    <h1>Welcome!<br />This a card memory game.</h1>
    <h3>
        You play this game by flipping cards over.<br />
        If the card values match they will stay flipped and your score will increase.<br />
        If they do not match the card will flip back over and you will lose a try.<br />
        After several tries you lose the game.<br />
        If you manage to flip all cards successfully you win!
    </h3>
    <button on:click={() => changeState('GAME_PLAY')}>Play Game</button>
</div>