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

    let player1Turn = false;

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
        ]

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
        ]

        // remove selected class from all pieces
        document.querySelectorAll(".pieces").forEach((piece) => {
            piece.classList.remove("selected");
        });

        // remove all possible move class
        clearPossibleMove();

        // reset player turn
        player1Turn = true;
    }

    // remove all possible move class
    function clearPossibleMove() {
        document.querySelectorAll(".possible-move").forEach((piece) => {
            piece.classList.remove("possible-move");
        });
    }

    /**
     * @param {any} event
     */
    function selectAndUnselect(event) {
        if (event.target.classList.contains("selected")) {
            // remove selected class
            event.target.classList.remove("selected");
            // remove all possible move class
            clearPossibleMove();
        } else {
            document.querySelectorAll(".pieces").forEach((piece) => {
                piece.classList.remove("selected");
            });
            // remove all possible move class
            clearPossibleMove();

            event.target.classList.add("selected");

            // re-render the possible move
            combinatorics(event);
        }
    }

    /**
     * @param {any} event
     */
    function preventSelectPiece(event) {
        // check and prevent player 1 from selecting player two
        if (
            player1Turn == true &&
            event.target.classList.contains("player-1")
        ) {
            combinatorics(event);
            // check if current piece is selected
            selectAndUnselect(event);
        } else if (
            player1Turn == false &&
            event.target.classList.contains("player-2")
        ) {
            combinatorics(event);
            selectAndUnselect(event);
        }
    }

    // calculate possible move for selected pieces
    /**
     * @param {any} event
     */
    function combinatorics(event) {
        /**
         * @type {string[]}
         */
        let index = [];

        // find index of class that contain piece- and split it to get its position in the board
        event.target.classList.forEach((/** @type {string} */ className) => {
            if (className.includes("piece-")) {
                index = className.split("-");
            }
        });

        // index[1] = current row, index[2] = current column
        let currentRow = parseInt(index[1]);
        let currentCol = parseInt(index[2]);

        let goDown = currentRow + 1;
        let goUp = currentRow - 1;
        let goLeft = currentCol - 1;
        let goRight = currentCol + 1;

        // create variable to store element go to left, right, up, down
        let elementGoDownLeft = document.querySelector(`.mn-${goDown}-${goLeft}`)
        let elementGoDownRight = document.querySelector(`.mn-${goDown}-${goRight}`)
        let elementGoUpLeft = document.querySelector(`.mn-${goUp}-${goLeft}`)
        let elementGoUpRight = document.querySelector(`.mn-${goUp}-${goRight}`)

        // check for player one to go down only if their selected piece is not a king
        if (player1Turn == true &&event.target.classList.contains("player-1")) {
            // calculate down left
            if (goDown <= 7 && pieces[goDown][goLeft] == 0 && goLeft >= 0) {
                if (elementGoDownLeft != null) {
                    elementGoDownLeft.classList.add("possible-move");
                }
            }

            // calculate down right
            if (goDown <= 7 && pieces[goDown][goRight] == 0 && goRight <= 9) {
                if (elementGoDownRight != null) {
                    elementGoDownRight.classList.add("possible-move");
                }
            }
            
            // if the selected piece is a king then we calculate the possible move from all direction
            if (event.target.classList.contains("king")) {
                // calculate up left
                if (goUp >= 0 && pieces[goUp][goLeft] == 0 && goLeft >= 0) {
                    if (elementGoUpLeft != null) {
                        elementGoUpLeft.classList.add("possible-move");
                    }
                }

                // calculate up right
                if (goUp >= 0 && pieces[goUp][goRight] == 0 && goRight <= 9) {
                    if (elementGoUpRight != null) {
                        elementGoUpRight.classList.add("possible-move");
                    }
                }
            }

        } else if (player1Turn == false && event.target.classList.contains("player-2")) {
            // calculate up left
            if (goUp >= 0 && pieces[goUp][goLeft] == 0 && goLeft >= 0) {
                if (elementGoUpLeft != null) {
                    elementGoUpLeft.classList.add("possible-move");
                }
            }

            // calculate up right
            if (goUp >= 0 && pieces[goUp][goRight] == 0 && goRight <= 9) {
                if (elementGoUpRight != null) {
                    elementGoUpRight.classList.add("possible-move");
                }
            }

            // if the selected piece is a king then we calculate the possible move from all direction
            if (event.target.classList.contains("king")) {
                // calculate down left
                if (goDown <= 7 && pieces[goDown][goLeft] == 0 && goLeft >= 0) {
                    if (elementGoDownLeft != null) {
                        elementGoDownLeft.classList.add("possible-move");
                    }
                }

                // calculate down right
                if (goDown <= 7 && pieces[goDown][goRight] == 0 && goRight <= 9) {
                    if (elementGoDownRight != null) {
                        elementGoDownRight.classList.add("possible-move");
                    }
                }
            }
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
                <div class="cell mn-{i}-{j}" class:dark={BOARD[i][j]}>
                    {#if pieces[i][j] == 1 || pieces[i][j] == 3}
                        <button
                            class="pieces piece-{i}-{j} player-1"
                            class:king={pieces[i][j] == 3}
                            on:click={preventSelectPiece}
                        />
                    {:else if pieces[i][j] == 2 || pieces[i][j] == 4}
                        <button
                            class="pieces piece-{i}-{j} player-2"
                            class:king={pieces[i][j] == 4}
                            on:click={preventSelectPiece}
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

    .possible-move {
        background-color: aquamarine;
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
