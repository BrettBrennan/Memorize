<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
    export let STATE;

    function changeState(state) {
        dispatch('changeState', { state });
    }

</script>
<style></style>
<div>
    <h1>Main Menu</h1>

    <button on:click={() => changeState('GAME_PLAY')}>Play Game</button>
</div>