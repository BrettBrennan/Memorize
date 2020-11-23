<script>
    export let boardSize;
    import Card from '../Cards/Card.svelte';

    let card_values = [ 'X', '#', '@', '$', '%', '&', 'O', '=', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H' ];
    let pairs = [];
    let currentValue = 0;

    let cards = [];
    let used_cards = new Array(boardSize - 1).fill(false);
    console.log(used_cards);
    for (let i = 0; i < boardSize; i++) {

        if (boardFilled()) break;
        if (!cardExists(i)){    
            let first_card = i;
            let second_card = getRandomCard(i);
            cards = [...cards, {
                id: i,
                value: card_values[currentValue],
            }];
            cards = [...cards, {
                id: second_card,
                value: card_values[currentValue],
            }];
            pairs = [...pairs, {
                id: i,
                first: first_card,
                second: second_card,
                value: card_values[currentValue],
            }];
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
        return (used_cards[id] === true);
    }
    function getRandomCard(duplicate) {
        let newCard = Math.floor(Math.random() * (boardSize - 0 + 1)) + 0;

        if (duplicate === newCard || cardExists(newCard)) return getRandomCard(duplicate);
        return newCard;
    }
    function selectCard(e) {
        console.log(e.detail.ID);
    }
    console.log(used_cards);
    console.log(pairs);
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