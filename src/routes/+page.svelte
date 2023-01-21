<!-- 
	@Author: Seakmeng Hor
	@Date: 1/21/2023
 -->

<!-- Script goes here -->
<script>
    // image source
    let src = {
        cat: 'Logo.jpg',
        dog: 'favicon.jpg'
    }

    // give default pieces position (discrete math usage: state machine)
    let pieces = [
        [0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
        [1, 0, 1, 0, 1, 0, 1, 0, 1, 0],
        [0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [2, 0, 2, 0, 2, 0, 2, 0, 2, 0],
        [0, 2, 0, 2, 0, 2, 0, 2, 0, 2],
        [2, 0, 2, 0, 2, 0, 2, 0, 2, 0],
    ];
    // set default 1dx8 board
    const boardN = 10;
    const boardM = 8;

</script>

<!-- Script end here -->

<!-- HTML goes here -->
<div class="container">
        <div class="board">
            {#each pieces as arrPiece, j}
            <div class="board-{j} board-row">
                {#each arrPiece as piece, i }
                    {#if piece == 1 || piece == 2}
                        <img class="piece-board piece-{piece}" src={src.cat} alt="cat">
                        <!-- <div class="piece-board piece-{piece}"></div> -->
                    {:else}
                        <img class="piece-board piece-{piece}" src={src.dog} alt="dog">
                        <!-- <div class="piece-board piece-{piece}"></div> -->
                    {/if}
                {/each}
            </div>
            {/each}
        </div>
</div>
<!-- HTML end here -->


<!-- Style end here  -->

<!-- Style goes here  -->
<style>
    .container {
        width: 100%;
        height: 100vh;
        margin: 0 auto;
    }

    .board {
        display: flex;
        flex-direction: column;
    }

    .board-row {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .piece-board {
        border: 1px solid greenyellow;
    }

    .piece-0 {
        width: 5%;
        height: auto;
        background: rgb(77, 56, 56);
    }

    .piece-2, .piece-1 {
        width: 5%;
        height: auto;
        background: wheat;
    }

</style>
