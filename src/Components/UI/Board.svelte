<script>
    export let boardSize;
    import Card from '../Cards/Card.svelte';

    let card_values = [ 'X', '#', '@', '$', '%', '&', 'O', '=', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H' ];
    let currentValue = 0;

    let cards = new Array(boardSize).fill({});
    let used_cards = new Array(boardSize).fill(false);
    // console.log(used_cards);
    console.log(`Board Size: ${boardSize}`);
    console.log(`Cards Length: ${cards.length}`);
    console.log(`Used Card Length: ${used_cards.length}`);
    for (let i = 0; i < boardSize / 2; i++) {
        if (!cardExists(i)){
            let value =card_values[currentValue]; 
            let second_card = getRandomCard(i);
            cards[i] = {
                id: i,
                value,                
            }
            cards[second_card] = {
                id: second_card,
                value,
            };
            currentValue++;
            used_cards[i] = true;
            used_cards[second_card] = true;
        }
    }
    function boardFilled () {
        let counter = 0;
        for (let i = 0; i < used_cards.length; i++)
            if (used_cards[i]) counter++;
        
        return counter === boardSize;
    }
    function cardExists(id) {
        return (used_cards[id]);
    }
    function getRandomCard(duplicate) {
        let newCard = Math.floor(Math.random() * ((boardSize - 1) - 0 + 1)) + 0;

        if (duplicate === newCard || cardExists(newCard)) return getRandomCard(duplicate);
        return newCard;
    }
    function selectCard(e) {
        console.log(e.detail.ID);
    }
    console.log({cards});
    // console.log(used_cards);
    // console.log(pairs);
</script>

<style>
    .board { 
        padding: 100px;
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
    }
</style>
<div class='board'> 
    {#each cards as card}
        <Card Value={card.value} ID={card.id} on:selectCard={selectCard} />
    {/each}
</div>