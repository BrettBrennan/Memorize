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
        width: 150px;
        height: 150px;
        margin: 20px 0;
        perspective: 300px;
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
        line-height: 150px;
    }
    .card_face-front {
		background-image: linear-gradient(120deg, #d4fc79 0%, #96e6a1 100%);
    }

    .card_face-back {
        text-align: center;
        color: white;
        background-image: linear-gradient(to top, #4facfe 0%, #00f2fe 100%);
        transform: rotateY( 180deg );
    }
    .card.is-flipped {
        transform: rotateY(180deg);
    }
</style>
<div class='container'>
    <div class='card' class:is-flipped={flipped} on:click={() => select()}>
        <div class="card_face card_face-front">?</div>
        <div class="card_face card_face-back">{flipped ? Value : ''}</div>
    </div>
</div>