<script>
    let flipped = true;
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
        background: red;
    }

    .card_face-back {
        background: rgb(104, 56, 24);
        transform: rotateY( 180deg );
    }
    .card.is-flipped {
        transform: rotateY(180deg);
    }
</style>
<div class='container'>
    <div class='card' class:is-flipped={flipped} on:click={() => flipped = !flipped}>
        <div class="card_face card_face-front">front</div>
        <div class="card_face card_face-back">back</div>
    </div>
</div>