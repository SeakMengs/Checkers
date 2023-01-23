<!-- 
    @Author: Seakmeng Hor
    @Date: 1/21/2023
 -->

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
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [2, 0, 2, 0, 2, 0, 2, 0, 2, 0],
        [0, 2, 0, 2, 0, 2, 0, 2, 0, 2],
        [2, 0, 2, 0, 2, 0, 2, 0, 2, 0],
    ];

    // reset pieces to default
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

        // remove selected class from all pieces
        document.querySelectorAll(".pieces").forEach((piece) => {
        piece.classList.remove("selected");
        });

        // reset player turn
        player1Turn = true;
    }

    let player1Turn = true;

    /**
     * @param {any} event
     */
    function validateSelected(event) {
        if (event.target.classList.contains("selected")) {
                // remove selected class
                event.target.classList.remove("selected");
            } else {
                document.querySelectorAll(".pieces").forEach((piece) => {
                    piece.classList.remove("selected")
                });
                event.target.classList.add("selected");
        }
    }

    /**
     * @param {any} event
     */
    function selectPiece(event) {
        // remove selected class from all pieces
        
        // check and prevent player 1 from selecting player two
        if (player1Turn == true && event.target.classList.contains("player-1")) {
            // check if current piece is selected
            validateSelected(event);
        } else if (player1Turn == false && event.target.classList.contains("player-2")) {
            validateSelected(event);
        }
    }

</script>
<!-- Script end here -->

<!-- HTML goes here -->
<div class="container">
    <div class="Content">
        <span>Player {player1Turn == true ? "one" : "two"}'s turn</span>
    </div>
    <!-- render board and piece -->
    <div class="board">
        {#each BOARD as row, i}
            {#each row as col, j}
                <div class="cell m{i} n{j}" class:dark={BOARD[i][j]}>
                    {#if pieces[i][j] == 1 || pieces[i][j] == 3}
                        <button
                            class="pieces piece-{i}-{j} player-1"
                            class:king={pieces[i][j] == 3}
                            on:click={selectPiece}
                        />
                    {:else if pieces[i][j] == 2 || pieces[i][j] == 4}
                        <button
                            class="pieces piece-{i}-{j} player-2"
                            class:king={pieces[i][j] == 4}
                            on:click={selectPiece}
                        />
                    {/if}
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
        margin: 0 auto;
        display: grid;
        place-items: center;
    }
    
    .board {
        display: grid;
        grid-template-columns: repeat(10, 5rem);
        border-radius: 0.5rem solid white;
        box-shadow: 0 0 10px 5px #ffffff;
    }
    
    .cell {
        width: 5rem;
        height: 5rem;
        background-color: #ee1b24;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .dark {
        background-color: #251e21;
    }
    
    .pieces {
        width: 4rem;
        height: 4rem;
        border-radius: 100%;
        border: none;
    }
    
    .player-1 {
        background-color: rgb(164, 164, 192);
    }
    
    .player-2 {
        background-color: rgb(248, 147, 129);
    }
    
    .selected {
        box-shadow: 0 0 10px 5px #ffffff;
    }

    /* responsive for mobile */
    @media (max-width: 768px) {
        .board {
            grid-template-columns: repeat(10, 2.2rem);
        }
        .cell {
            width: 2.2rem;
            height: 2.2rem;
        }
        .pieces {
            width: 1.3rem;
            height: 1.3rem;
        }
    }
    </style>
<!-- Style end here  -->
