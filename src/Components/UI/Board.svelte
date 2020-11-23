<script>
    export let boardSize;
    import Card from '../Cards/Card.svelte';
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
    <Card />
</div>