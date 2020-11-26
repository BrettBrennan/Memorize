<script>
    import Card from '../Cards/Card.svelte';
    export let boardSize;
    export const restartGame = () => {
        generateCards();
    }; 
    let card_values = [ 'X', '#', '@', '$', '%', '&', '=', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'Y', 'Z'];
    let currentValue;
    let cards;
    let used_cards;
    let first;
    let second;
    let canFlipCards = true;
    let tries = 3;
    let score = 0;
    let won = false;
    let BOARD_STATE = 'PLAYING';
    async function generateCards() {
        BOARD_STATE = 'PLAYING';
        won = false;
        score = 0;
        tries = 3;
        currentValue = 0;
        first = -1;
        second = -1;
        cards = new Array(boardSize).fill({});
        used_cards = new Array(boardSize).fill(false);
        for (let i = 0; i < boardSize; i++) {
            if (!cardExists(i)){
                let value =card_values[currentValue]; 
                let second_card = await getRandomCard(i);
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
    function boardCheck() {
        let nonFlipped = 0;
        for (let card in cards) {
            if (!cards[card].flipped) nonFlipped += 1;
        }

        if (nonFlipped === 0) {
            // User has won!
            won = true;
            BOARD_STATE = 'GAME_OVER';
        }else{
            // Continue Game.
        }
    }
    function matchCheck() {
        if (first === -1 || second === -1) return;
        setTimeout(() => {
            if (cards[first].value === cards[second].value){ // They Match!
                cards[first].flipped = true;
                cards[first].flippable = false;
                cards[second].flipped = true;
                cards[second].flippable = false;
                score += 50;
                boardCheck();
            }else{ // They do not match
                cards[first].flipped = false;
                cards[second].flipped = false;
                tries--;
            }
            first = -1;
            second = -1;    
            canFlipCards = true;


            if (tries <= 0) {
                won = false;
                BOARD_STATE = 'GAME_OVER';
            }
        }, 1000);
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
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(175px, 1fr));
        justify-items: center;
    }
    h2 {
        color: #e5e5e5;

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
</style>
{#if BOARD_STATE === 'PLAYING'}
<h2>Score: {score} Tries Left: {tries} </h2>
<div class='board'> 
    {#each cards as card}
        <Card Value={card.value} ID={card.id} canFlip={canFlipCards ? card.flippable : false} flipped={card.flipped} on:selectCard={selectCard} />
    {/each}
</div>
{:else if BOARD_STATE === 'GAME_OVER'}
    {#if won === true}
        <h2>Congratulations! You won! Final Score: {score}</h2>
    {:else}
        <h2>Oh no! You lost! Final Score: {score}</h2>
    {/if}
    <button on:click={() => generateCards()}>Play Again?</button>
{/if}