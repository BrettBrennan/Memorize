<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    export let Value;
    export let ID;
    export let canFlip;
    export let flipped = false;
    

    function select() {
        if (canFlip)
            flipped = true;
        dispatch('selectCard', { ID });
    }
</script>
<style>
    .container {
        width: 200px;
        height: 260px;
        border: 1px solid #CCC;
        margin: 40px 0;
        perspective: 600px;
    }
    .card {
        cursor: pointer;
        text-align: center;
        font-size: 3rem;
        color: #333;
        width: 100%;
        height: 100%;
        position: relative;
        transition: transform 0.7s;
        transform-style: preserve-3d;
    }
    .card_face {
        position: absolute;
        height: 100%;
        width: 100%;
        backface-visibility: hidden;
    }
    .card_face-front {
        background: rgb(104, 56, 24);
    }

    .card_face-back {
        background: rgb(199, 247, 255);
        transform: rotateY( 180deg );
    }
    .card.is-flipped {
        transform: rotateY(180deg);
    }
</style>
<div class='container'>
    <div class='card' class:is-flipped={flipped} on:click={() => select()}>
        <div class="card_face card_face-front">{ID}</div>
        <div class="card_face card_face-back">{flipped ? Value : ''}</div>
    </div>
</div>