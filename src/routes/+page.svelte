<!-- Script goes here -->
<script>
    // image source
    let src = {
        cat: "Logo.jpg",
        dog: "favicon.jpg",
    };

    const BOARD = [
        [0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
        [1, 0, 1, 0, 1, 0, 1, 0, 1, 0],
        [0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
        [1, 0, 1, 0, 1, 0, 1, 0, 1, 0],
        [0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
        [1, 0, 1, 0, 1, 0, 1, 0, 1, 0],
        [0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
        [1, 0, 1, 0, 1, 0, 1, 0, 1, 0],
    ];

    // give default pieces position (discrete math usage: state machine)
    // 1 = player 1, 2 = player 2, 0 = empty, 3 = player 1 king, 4 = player 2 king
    let pieces = [
        [0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
        [1, 0, 1, 0, 1, 0, 1, 0, 1, 0],
        [0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
        [1, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [2, 0, 2, 0, 2, 0, 2, 0, 2, 0],
        [0, 2, 0, 2, 0, 2, 0, 2, 0, 2],
        [2, 0, 2, 0, 2, 0, 2, 0, 2, 0],
    ];

    // reset board to default
    function resetGame() {
        pieces = [
            [0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
            [1, 0, 1, 0, 1, 0, 1, 0, 1, 0],
            [0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
            [2, 0, 2, 0, 2, 0, 2, 0, 2, 0],
            [0, 2, 0, 2, 0, 2, 0, 2, 0, 2],
            [2, 0, 2, 0, 2, 0, 2, 0, 2, 0],
        ];
    }
</script>

<!-- 
	@Author: Seakmeng Hor
	@Date: 1/21/2023
 -->


<!-- Script end here -->

<!-- HTML goes here -->
<div class="container">
    <!-- <div class="board">
        {#each pieces as arrPiece, j}
        <div class="board-{j} board-row">
            {#each arrPiece as piece, i }
                {#if piece == 1 || piece == 2}
                    <img class="piece-board piece-{piece}" src={src.cat} alt="cat">
                    <div class="piece-board piece-{piece}"></div>
                {:else}
                    <img class="piece-board piece-{piece}" src={src.dog} alt="dog">
                    <div class="piece-board piece-{piece}"></div>
                {/if}
            {/each}
        </div>
        {/each}
    </div> -->
    <div class="board">
        {#each BOARD as row, i}
            {#each row as col, j}
            <div class="cell" class:red={BOARD[i][j]}>
                    
            </div>
            {/each}
        {/each}
    </div>
    <button on:click={resetGame}>Restart the game</button>
</div>
<!-- HTML end here -->

<!-- Style goes here  -->
<style>
    .container {
        width: 100%;
        height: 100vh;
        display: grid;
        place-items: center;
    }
    .board {
        display: grid;
        grid-template-columns: repeat(10, 5rem);
        border: 1px solid white;
    }

    .cell {
        width: 5rem;
        height: 5rem;
        background-color: #251e21;
    }
    .red {
        background-color: #ee1b24;
    }
</style>
<!-- Style end here  -->