<!-- Script goes here -->
<script>
    // image source
    // let src = {
    //     logo: "logo.jpg",
    //     king: "king.png",
    // };

    let player1Turn = true;
    let playerOneScore = 0;
    let playerTwoScore = 0;

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

    // set up win (for debugging)
    // let pieces = [
    //     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    //     [1, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    //     [0, 0, 0, 0, 0, 0, 0, 3, 0, 0],
    //     [0, 0, 1, 0, 0, 0, 0, 0, 0, 0],
    //     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    //     [0, 0, 0, 0, 2, 0, 0, 0, 0, 0],
    //     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    //     [0, 0, 0, 0, 4, 0, 0, 0, 0, 0],
    // ];
    // let pieces = [
    //     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    //     [4, 0, 4, 0, 0, 0, 0, 0, 0, 0],
    //     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    //     [0, 0, 4, 0, 0, 0, 0, 0, 0, 0],
    //     [0, 0, 0, 3, 0, 0, 0, 0, 0, 0],
    //     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    //     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    //     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    // ];

    checkPlayerScore();

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

        // remove all possible move class
        clearPossibleMove();

        // reset player turn
        player1Turn = true;
        checkPlayerScore();
    }

    // switch player turn
    function switchPlayer() {
        player1Turn = !player1Turn;
    }

    function checkPlayerScore() {
        playerOneScore = 0;
        playerTwoScore = 0;

        for (let i = 0; i < pieces.length; i++) {
            for (let j = 0; j < pieces[i].length; j++) {
                if (pieces[i][j] == 1 || pieces[i][j] == 3) {
                    playerOneScore++;
                } else if (pieces[i][j] == 2 || pieces[i][j] == 4) {
                    playerTwoScore++;
                }
            }
        }
    }

    // remove all possible move class
    function clearPossibleMove() {
        document.querySelectorAll(".possible-move").forEach((piece) => {
            // change child class from hiddenMovable to movable
            piece.children[0].classList.remove("movable");
            piece.children[0].classList.add("hiddenMovable");

            // hide movable element
            // @ts-ignore
            piece.children[0].style.display = "none";
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
            // remove all selected class from all pieces before adding selected class to current piece
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
        // create the variable to avoid long code
        let containPlayer1 = event.target.classList.contains("player-1");
        let containPlayer2 = event.target.classList.contains("player-2");

        // check and prevent player 1 from selecting player two
        if (player1Turn == true && containPlayer1) {
            combinatorics(event);
            // check if current piece is selected
            selectAndUnselect(event);
        } else if (player1Turn == false && containPlayer2) {
            combinatorics(event);
            selectAndUnselect(event);
        }
    }

    /**
     * @param {any} element
     * @param {string | undefined} [move]
     */
    function checkCanKill(element, move) {
        let index = element.classList[1].split("-");
        let newElement = null;

        if (move == "goDownLeft") {
            newElement = document.querySelectorAll(
                `.mn-${parseInt(index[1]) + 1}-${parseInt(index[2]) - 1}`
            );
        } else if (move == "goDownRight") {
            newElement = document.querySelectorAll(
                `.mn-${parseInt(index[1]) + 1}-${parseInt(index[2]) + 1}`
            );
        } else if (move == "goUpLeft") {
            newElement = document.querySelectorAll(
                `.mn-${parseInt(index[1]) - 1}-${parseInt(index[2]) - 1}`
            );
        } else if (move == "goUpRight") {
            newElement = document.querySelectorAll(
                `.mn-${parseInt(index[1]) - 1}-${parseInt(index[2]) + 1}`
            );
        }

        if (newElement == null) {
            return false;
        }

        return newElement;
    }

    /**
     * @param {any} element
     * @param {string | undefined} [move]
     */
    function addPossibleMove(element, move) {
        if (element != null) {
            // check if element is a piece, if so we check kill move, if no kill possible we stop the function
            if (element.children[0].classList.contains("pieces")) {
                let oldElement = element;
                /**
                 * @type {any[]}
                 */
                let index = [];

                if (checkCanKill(element, move) != false) {
                    element = checkCanKill(element, move);
                    element = element[0];
                }

                if (element == null) {
                    return;
                }

                index = element.classList[1].split("-");

                // check if the position is piece, if peace we stop the function
                if (pieces[index[1]][index[2]] != 0) {
                    return;
                }
                // if the piece is the same as player piece we stop the function
                if (
                    player1Turn == true &&
                    oldElement.children[0].classList.contains("player-1")
                ) {
                    return;
                } else if (
                    player1Turn == false &&
                    oldElement.children[0].classList.contains("player-2")
                ) {
                    return;
                }
            }

            element.classList.add("possible-move");

            // change child class from hiddenMovable to movable
            element.children[0].classList.remove("hiddenMovable");
            element.children[0].classList.add("movable");

            // add move function to child element
            element.children[0].addEventListener("click", movePiece);

            // display child element
            element.children[0].style.display = "block";
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

        // create variable to store element go to left, right, up, down (for readability)
        let elementGoDownLeft = document.querySelector(
            `.mn-${goDown}-${goLeft}`
        );

        let elementGoDownRight = document.querySelector(
            `.mn-${goDown}-${goRight}`
        );

        let elementGoUpLeft = document.querySelector(`.mn-${goUp}-${goLeft}`);

        let elementGoUpRight = document.querySelector(`.mn-${goUp}-${goRight}`);

        // create the variable to avoid long code
        let containPlayer1 = event.target.classList.contains("player-1");
        let containPlayer2 = event.target.classList.contains("player-2");

        // check for player one to go down only if their selected piece is not a king
        if (player1Turn == true && containPlayer1) {
            // calculate down left
            addPossibleMove(elementGoDownLeft, "goDownLeft");

            // calculate down right
            addPossibleMove(elementGoDownRight, "goDownRight");

            // if the selected piece is a king then we calculate the possible move from all direction
            if (event.target.classList.contains("king")) {
                // calculate up left
                addPossibleMove(elementGoUpLeft, "goUpLeft");

                // calculate up right
                addPossibleMove(elementGoUpRight, "goUpRight");
            }
        } else if (player1Turn == false && containPlayer2) {
            // calculate up left
            addPossibleMove(elementGoUpLeft, "goUpLeft");

            // calculate up right
            addPossibleMove(elementGoUpRight, "goUpRight");

            // if the selected piece is a king then we calculate the possible move from all direction
            if (event.target.classList.contains("king")) {
                // calculate down left
                addPossibleMove(elementGoDownLeft, "goDownLeft");

                // calculate down right
                addPossibleMove(elementGoDownRight, "goDownRight");
            }
        }
    }

    /**
     * @param {any} curRow
     * @param {any} curCol
     */
    function findSelectedLocation(curRow, curCol) {
        // get their children
        let element = document.querySelector(`.mn-${curRow}-${curCol}`)?.children[0];

        // check if it's a selected piece
        if (element != null) {
            if (element.classList.contains("selected")) {
                // console.log("Found a piece at location " + m.toString() + ", " + n.toString())
                return true;
            }
        }

        return false;
    }

    /**
     * @param {number} curRow
     * @param {number} curCol
     */
    function checkKing(curRow, curCol) {
        // check if the piece is a king
        if (player1Turn) {
            if (curRow == 7) {
                pieces[curRow][curCol] = 3;
                return true;
            }
        } else {
            // console.log("player two " + curRow.toString() + ' ' + curCol.toString())
            if (curRow == 0) {
                pieces[curRow][curCol] = 4;
                // console.log(pieces[curRow][curCol])
                return true;
            }
        }

        // console.log(pieces[curRow][curCol])
        return false;
    }

    /**
     * @param {number} currentRow
     * @param {number} currentCol
     * @param {number} minusRow
     * @param {number} minusCol
     */
    function isKingMove(currentRow, currentCol, minusRow, minusCol) {
        // check if king move
        if (
            pieces[currentRow + minusRow][currentCol + minusCol] == 3 &&
            player1Turn
        ) {
            pieces[currentRow][currentCol] = 3;
            return true;
        } else if (
            pieces[currentRow + minusRow][currentCol + minusCol] == 4 &&
            !player1Turn
        ) {
            pieces[currentRow][currentCol] = 4;
            return true;
        }

        let playerNumber = NaN;
        // check which player turn it is
        player1Turn ? (playerNumber = 1) : (playerNumber = 2);

        // if not king then give the piece the player number
        pieces[currentRow][currentCol] = playerNumber;

        return false;
    }

    /**
     * @param {any} event
     */
    function movePiece(event) {
        /**
         * @type {string[]}
         */
        let index = [];

        // find index of parent element class that contain mn- and split it to get its position in the board
        event.target.parentElement.classList.forEach(
            (/** @type {string} */ className) => {
                if (className.includes("mn-")) {
                    index = className.split("-");
                }
            }
        );

        // index[1] = current row, index[2] = current column
        let currentRow = parseInt(index[1]);
        let currentCol = parseInt(index[2]);

        // [0] = up left, [1] = up right, [2] = down left, [3] = down right
        let findSelectedOrigin = [
            findSelectedLocation(currentRow - 1, currentCol - 1),
            findSelectedLocation(currentRow - 1, currentCol + 1),
            findSelectedLocation(currentRow + 1, currentCol - 1),
            findSelectedLocation(currentRow + 1, currentCol + 1),

            // start checking for kill move
            findSelectedLocation(currentRow - 2, currentCol - 2),
            findSelectedLocation(currentRow - 2, currentCol + 2),
            findSelectedLocation(currentRow + 2, currentCol - 2),
            findSelectedLocation(currentRow + 2, currentCol + 2),
        ];

        for (let i = 0; i < findSelectedOrigin.length; i++) {
            if (findSelectedOrigin[i] == true) {
                if (i == 0) {
                    // check if king move (up left)
                    let isKing = isKingMove(currentRow, currentCol, -1, -1);

                    // change selected piece to movable space
                    pieces[currentRow - 1][currentCol - 1] = 0;

                    if (isKing == false) {
                        checkKing(currentRow, currentCol);
                    }
                } else if (i == 1) {
                    // check if king move (up right)
                    let isKing = isKingMove(currentRow, currentCol, -1, 1);

                    // change selected piece to movable space
                    pieces[currentRow - 1][currentCol + 1] = 0;

                    if (isKing == false) {
                        checkKing(currentRow, currentCol);
                    }
                } else if (i == 2) {
                    // check if king move (down left)
                    let isKing = isKingMove(currentRow, currentCol, 1, -1);

                    // change selected piece to movable space
                    pieces[currentRow + 1][currentCol - 1] = 0;

                    if (isKing == false) {
                        checkKing(currentRow, currentCol);
                    }
                } else if (i == 3) {
                    // check if king move (down right)
                    let isKing = isKingMove(currentRow, currentCol, 1, 1);

                    // change selected piece to movable space
                    pieces[currentRow + 1][currentCol + 1] = 0;

                    if (isKing == false) {
                        checkKing(currentRow, currentCol);
                    }
                } else if (i == 4) {
                    // check if king move (up up left left)
                    let isKing = isKingMove(currentRow, currentCol, -2, -2);

                    // change selected piece to movable space
                    pieces[currentRow - 2][currentCol - 2] = 0;

                    // remove the piece that was jumped over
                    pieces[currentRow - 1][currentCol - 1] = 0;

                    if (isKing == false) {
                        checkKing(currentRow, currentCol);
                    }
                } else if (i == 5) {
                    // check if king move (up up right right)
                    let isKing = isKingMove(currentRow, currentCol, -2, 2);

                    // change selected piece to movable space
                    pieces[currentRow - 2][currentCol + 2] = 0;

                    // remove the piece that was jumped over
                    pieces[currentRow - 1][currentCol + 1] = 0;

                    if (isKing == false) {
                        checkKing(currentRow, currentCol);
                    }
                } else if (i == 6) {
                    // check if king move (down down left left)
                    let isKing = isKingMove(currentRow, currentCol, 2, -2);

                    // change selected piece to movable space
                    pieces[currentRow + 2][currentCol - 2] = 0;

                    // remove the piece that was jumped over
                    pieces[currentRow + 1][currentCol - 1] = 0;

                    if (isKing == false) {
                        checkKing(currentRow, currentCol);
                    }
                } else if (i == 7) {
                    // check if king move (down down right right)
                    let isKing = isKingMove(currentRow, currentCol, 2, 2);

                    // change selected piece to movable space
                    pieces[currentRow + 2][currentCol + 2] = 0;

                    // remove the piece that was jumped over
                    pieces[currentRow + 1][currentCol + 1] = 0;

                    if (isKing == false) {
                        checkKing(currentRow, currentCol);
                    }
                }
                break;
            }
        }

        // clear possible move
        clearPossibleMove();

        // switch player turn
        switchPlayer();

        // check score
        checkPlayerScore();
    }
</script>

<!-- Script end here -->

<!-- HTML goes here -->
<div class="container">
    <div class="content">
        <span class:turn={!player1Turn}>Player one</span>
        <span class="wall">|</span>
        <span class:turn={player1Turn}>Player Two</span>
    </div>
    <!-- render board and piece -->
    <div class="board">
        {#if playerOneScore == 0}
            <span class="playerWinMessage"> Player two wins! </span>
        {:else if playerTwoScore == 0}
            <span class="playerWinMessage"> Player one wins! </span>
        {/if}
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
                    {:else}
                        <button class="hiddenMovable" style="display: none;" />
                    {/if}
                </div>
            {/each}
        {/each}
    </div>
    <span>Player one {playerOneScore} - {playerTwoScore} Player two</span>
    <button on:click={resetGame} class="game-btn">Restart the game </button>
    <a
        href="https://www.youtube.com/watch?v=MOW9k_C4vFU&ab_channel=wikiHow"
        target="blank"
        class="game-btn">How to play</a
    >
    <a class="github-source credit" href="https://github.com/SeakMengs/Checker" target="blank">
        <img class="profile" src="https://avatars.githubusercontent.com/u/54373229?v=4" alt="profile">
        <a class="credit" href="https://github.com/SeakMengs/Checkers" target="blank">Created by @Seakmeng | Click to see source code</a>
    </a>
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
        position: relative;
        display: grid;
        grid-template-columns: repeat(10, 5rem);
        border-radius: 0.5rem solid white;
        box-shadow: 0 0 20px 5px #202020;
    }

    .cell {
        width: 5rem;
        height: 5rem;
        /* background-color: #ee1b24; */
        background-color: rgba(237, 61, 67, 0.80);
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .dark {
        background-color: rgba(37, 30, 33, 0.80);
    }

    .pieces {
        width: 4rem;
        height: 4rem;
        border-radius: 100%;
        border: none;
        outline: none;
        animation: rotate 0.5s linear;
    }

    .player-1 {
        /* background-color: rgb(164, 164, 192); */
        background: radial-gradient(
            circle at 65% 15%,
            white 1px,
            #4f7272 3%,
            #a4a4c0 60%,
            black 100%
        );
    }

    .player-2 {
        /* background-color: rgb(248, 147, 129); */
        /* background: radial-gradient(circle at 65% 15%, white 1px, #f89381 3%, #f89381 60%, black 100%); */
        background: radial-gradient(
            circle at 65% 15%,
            white 1px,
            rgb(124 71 61) 3%,
            #f89381 60%,
            black 100%
        );
    }

    .content {
        display: flex;
        flex-direction: row;
        text-align: center;
        align-items: center;
        justify-content: space-between;
    }

    .wall {
        font-size: 50px;
        margin-left: 15px;
        margin-right: 15px;
    }

    .turn {
        color: rgba(245,222,179, 0.1);
    }

    .selected {
        box-shadow: 0 0 10px 5px #ffffff;
    }

    span {
        font-size: 2rem;
        font-weight: 700;
        margin: 1rem;
        user-select: none;
    }

    .game-btn {
        font-size: 2rem;
        text-align: center;
        display: inline-block;
        margin: 5px;
        font-weight: bold;
        padding: 10px 10px 10px 10px;
        background-color: lightgray;
        text-shadow: -1px -1px black, 1px 1px white;
        color: gray;
        -webkit-border-radius: 7px;
        -moz-border-radius: 7px;
        -o-border-radius: 7px;
        border-radius: 7px;
        box-shadow: 0 0.2em gray;
        cursor: pointer;
        user-select: none;
        text-decoration: none;
        border: none;
        font-family: "Courier New", Courier, monospace;
    }

    .game-btn:active {
        box-shadow: none;
        position: relative;
        top: 0.2em;
    }

    /* make a winner drop down animation and give playerWin class background on top of all element */
    .playerWinMessage {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        font-size: 5.4rem;
        font-weight: 700;
        animation: playerWin 2s ease;
        background-color: rgba(0, 0, 0, 0.9);
        width: 100%;
        text-align: center;
        margin: 0;
    }

    .github-source {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .credit {
        padding: 1rem;
        color: wheat;
        text-decoration: none;
    }

    .profile {
        width: 50px;
        height: 50px;
        border-radius: 50%;
    }

    @keyframes playerWin {
        0% {
            opacity: 0;
            transform: translate(-50%, -50%);
        }
        100% {
            opacity: 1;
            transform: translate(-50%, -50%);
        }
    }

    @keyframes rotate {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }

    /* responsive for mobile */
    @media only screen and (max-width: 600px) {
        .board {
            grid-template-columns: repeat(10, 4rem);
        }
        .pieces {
            width: 3rem;
            height: 3rem;
        }
        .cell {
            width: 4rem;
            height: 4rem;
        }
        .wall {
            font-size: 40px;
        }
        .turn {
            font-size: 1.5rem;
        }
        .game-btn {
            font-size: 1.5rem;
        }
        .playerWinMessage {
            font-size: 4rem;
        }
        .credit {
            font-size: 10px;
        }
        .movable {
            width: 3rem;
            height: 3rem;
        }
        .profile {
            width: 40px;
            height: 40px;
        }
        span {
            font-size: 1.5rem;
        }
    }

</style>
<!-- Style end here  -->