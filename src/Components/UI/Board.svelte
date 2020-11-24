<script>
    export let boardSize;
    import Card from '../Cards/Card.svelte';

    let card_values = [ 'X', '#', '@', '$', '%', '&', 'O', '=', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H' ];
    let currentValue;
    let cards;
    let used_cards;
    let first;
    let second;
    let canFlipCards = true;
    
    async function generateCards() {
        currentValue = 0;
        first = -1;
        second = -1;
        cards = new Array(boardSize).fill({});
        used_cards = new Array(boardSize).fill(false);
        for (let i = 0; i < boardSize; i++) {
            if (!cardExists(i)){
                let value =card_values[currentValue]; 
                let second_card = await getRandomCard(i);
                // console.log(`first card ${i}, second card ${second_card}`);
                cards[i] = {
                    id: i,
                    value,      
                    flippable: true,
                    flipped: false,          
                }
                cards[second_card] = {
                    id: second_card,
                    value,
                    flippable: true,          
                    flipped: false,          
                };
                currentValue++;
                used_cards[i] = true;
                used_cards[second_card] = true;
            }
        }
        //console.log(cards, used_cards);
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
    async function getRandomCard(duplicate) {
        let newCard = Math.floor(Math.random() * ((boardSize - 1) - 0 + 1));

        if (duplicate === newCard || cardExists(newCard))
            newCard = await getRandomCard(duplicate);
        
        return newCard;
    }
    function matchCheck() {
        if (first === -1 || second === -1) return;

        setTimeout(() => {
            if (cards[first].value === cards[second].value){ // They Match!
                cards[first].flipped = true;
                cards[first].flippable = false;
                cards[second].flipped = true;
                cards[second].flippable = false;
            }else{ // They do not match
                cards[first].flipped = false;
                cards[second].flipped = false;
            }
            first = -1;
            second = -1;    
            canFlipCards = true;
        }, 2000);
    }
    function selectCard(e) {
        const cID = e.detail.ID;
        console.log(`Selecting card ${cID}`);
        if (cards[cID].flipped) return;

        if (cards[cID].flippable){
            if (first === -1){
                first = cID;
                cards[cID].flipped = true;
                return;
            }

            if (second === -1) {
                second = cID;
                cards[cID].flipped = true;
                canFlipCards = false;
                matchCheck();
                return;
            }
            
        }
    }

    generateCards();
</script>

<style>
    .board { 
        padding: 100px;
        display: grid;
        grid-template-columns: repeat(4, 1fr);
    }
</style>
<button on:click={() => generateCards()}>Regen</button>
<div class='board'> 
    {#each cards as card}
        <Card Value={card.value} ID={card.id} canFlip={canFlipCards ? card.flippable : false} flipped={card.flipped} on:selectCard={selectCard} />
    {/each}
</div>