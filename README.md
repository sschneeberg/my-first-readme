# TIC TAC TOE README
Creating a proper README.md for `tic-tac-toe`

## To Install on local computer
1. Go to [repo](https://github.com/sschneeberg/tic-tac-toe) on Github profile
2. `fork` and `clone` repo
3. clone to local machine
``` text 
git clone http://github.com/sschneeberg/tic-tac-toe.git
```
4. Go to `tic-tac-toe` directory
5. Open files in code editor to edit
6. Run `index.html` in browser to play

# Code Break Down

## HTML and CSS GRID

The html structure is primarily a series of cell divisions, each labeled with a string corresponding to a cell of a 2D array in the javascript file. 

```html
<div class="board">
    <div class="cell" id="c00">
        <p></p>
    </div>
    <div class="cell" id="c01">
        <p></p>
    </div>
    <div class="cell" id="c02">
        <p></p>
    </div>
    <div class="cell" id="c10">
        <p></p>
    </div>
    <div class="cell" id="c11">
        <p></p>
    </div>
    <div class="cell" id="c12">
        <p></p>
    </div>
    <div class="cell" id="c20">
        <p></p>
    </div>
    <div class="cell" id="c21">
        <p></p>
    </div>
    <div class="cell" id="c22">
        <p></p>
    </div>
</div>
```
The CSS primarily functions to formats the board into a grid that the user can click in to play. 

```css
.board {
    visibility: hidden;
    position: relative;
    display: grid;
    grid-template-rows: repeat(3, 1fr);
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 1em;
    width: 50vw;
    height: 50vw;
    margin: 1rem auto;
}
```
A start screen and a reset screen appear before and after the game allowing the player to chose or change the mode of play. The class `show` is toggled when `gameOver` is triggered in javascript, to ensure these screens only appear at the right points in play.

```css
.resetScreen,
.startScreen {
    position: absolute;
    top: 50%;
    right: 50%;
    transform: translate(50%, 0);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    visibility: hidden;
    height: 30vh;
    width: 40vw;
    background: rgb(241, 168, 205);
    border: 1px solid white;
    font-family: 'Amatic SC', cursive;
    font-size: 2.5em;
}

.show {
    visibility: visible;
}
```

## Javascript Functionality

Two main functions govern the tic-tac-toe gameplay: 
1. `assignGamepiece(e)` runs on `click` and places the player's piece in both the appropriate 2D array cell and within the correct html div tag. It ensures this cannot happen when it is not the player's turn nor when the game is over.  After placing, the second major function is invoked.

```js 
function assignGamepiece(e) {
    //ensure computer does not get two turns by checking the mode+turn first
    if (opponent === 'local' || (opponent === 'computer' && turn === 'X')) {
        let cell = e.target.id;
        //do not try to assign a piece to the wrong html element
        if (cell === '') { return; }
        //do not play if game is over
        if (gameOver) { return; }
        //if x's turn, add x to game board and display in correct div, else if o's turn same for o
        //check if cell is occupied.
        let i = parseInt(cell[1]);
        let j = parseInt(cell[2]);
        if (gameBoard[i][j] === 0) {
            let index = computerTrys.indexOf(cell);
            computerTrys.splice(index, 1);
            //empty cell --> place piece, check win, change turn
            e.target.children[0].innerText = turn;
            gameBoard[i][j] = turn;
            checkEndConditions(i, j);
        }
    }
}
```

2. `checkEndConditions(i,j)` is called after each player's move (including the computer's turn in `computer` mode).  It calls a series of functions that check all possible tic-tac-toe win conditions as well as the posibility of a tie.  Upon discovering a win or a tie the proper end-game `resetScreen` is displayed with the correct win tallies updated.  If no end condition is detected, the function changes the turn, calling up the computer to move if the player is not playing in `local` mode.

```js
function checkEndConditions(i, j) {
    //check win and tie otherwise, change the turn and carry on
    if (checkWin(i, j)) {
        //display win screen
        console.log('win');
        gameOver = true;
        let screen = document.querySelector('.resetScreen');
        if (turn === 'X') {
            playerWins += 1;
            screen.children[0].innerText = `${turn} wins!\n ${turn} has won ${playerWins} time(s)`;
        } else {
            opponentWins += 1;
            if (opponent === 'local') {
                screen.children[0].innerText = `${turn} wins!\n ${turn} has won ${opponentWins} time(s)`;
            } else {
                screen.children[0].innerText = `The computer wins!\n Computer has won ${opponentWins} time(s)`
            }
        }
        screen.classList.toggle('show');
    } else if (checkTie()) {
        //display end screen
        console.log('tie')
        gameOver = true;
        let screen = document.querySelector('.resetScreen');
        screen.children[0].innerText = "It's a tie!"
        screen.classList.toggle('show');
    } else {
        //change turn and continue playing
        if (turn === 'X') {
            turn = 'O';
            if (opponent === 'computer') { //computer will always be o
                //take computer's turn if it's the computer's turn
                document.querySelector('.turn').innerText = "Computers's turn";
                setTimeout(computerChoose, 1000);
            } else {
                document.querySelector('.turn').innerText = `${turn}'s turn`;
            }
        } else if (turn === 'O' && opponent === 'local') {
            turn = 'X';
            document.querySelector('.turn').innerText = `${turn}'s turn`
        }
    }
}
```


| Functions            | Description |
| -----------          | ----------- |
| `assignGamepiece(e)`        | Displays player's move |
| `checkEndConditions(i,j)`  | Checks for end-game after each turn |
| `checkWin(i,j)`        | Checks for a win |
| `checkCols(i)`  | Checks for a vertical win |
| `checkRows(j)`        | Checks for a horizontal win |
| `checkDiags(i,j)`  | Checks for a diagonal win |
| `checkTie()`        | Checks for a full board without a win |
| `computerChoose()`        | Chooses and displays computer's move in a random empty cell |
| `resetBoard()`  | Resets all html cells to empty and resets the correct Javascript trackers |
| `startGame()`        | Toggles the board into view and removes `startScreen` on `click` of start button|
| `setMode(e)`  | Sets the mode and resets win counters on `click` of mode buttons |
| `changeMode()`  | Sets the mode and resets win counters on 'click` of the change mode button |

&nbsp;
##  
&nbsp;

# Future Considerations
 Currently there are two modes to play: `local` (against your friends) or `computer` (against a bot).

 The computer algorithm is simply a random choice of remaining empty cells, making gameplay not challenging for the user.

 ```javascript
 function computerChoose() {
    //pick cell
    let index = Math.floor(Math.random() * computerTrys.length);
    //remove from remaining - this is also updated on player's turn
    let cell = computerTrys[index];
    computerTrys.splice(index, 1);
    //update gameboard and user interface
    document.getElementById(cell).children[0].innerText = turn;
    let i = parseInt(cell[1]);
    let j = parseInt(cell[2]);
    gameBoard[i][j] = turn;
    //check for wins, if none then change the turn
    checkEndConditions(i, j);
    turn = 'X';
    document.querySelector('.turn').innerText = `${turn}'s turn`
}
```

To improve this game, the computer algorithm should be optimized to win.

&nbsp;
##  
&nbsp;
## 
&nbsp;
##  
&nbsp;

# README Practice: 
The following was a practice exercise for writing a README.md
## Steps to Install on local computer
1. Go to [repo](https://github.com/sschneeberg/my-first-readme) on Github profile
2. `fork` and `clone` repo
3. clone to local machine
``` text 
git clone http://github.com/sschneeberg/my-first-readme.git
```
4. Go to `my-first-readme` directory
5. open `README.md` in code editor

# Code snippet examples
Code by Rome Bell
```javascript 
const handleWin = (letter) => {
  gameIsLive = false;
  if (letter === "x") {
    statusDiv.innerHTML = `${letterToSymbol(letter)} has won!`;
  } else {
    statusDiv.innerHTML = `<span>${letterToSymbol(letter)} has won!</span>`;
  }
};
```

```css
.grid {
    background-color: salmon;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    gap: 15px;
    margin-top: 50px;
}
```
```html
<div class="grid">
    <div class="box" id="box-1"></div>
    <div class="box" id="box-2"></div>
    <div class="box" id="box-3"></div>
    <div class="box" id="box-4"></div>
    <div class="box" id="box-5"></div>
    <div class="box" id="box-6"></div>
    <div class="box" id="box-7"></div>
    <div class="box" id="box-8"></div>
    <div class="box" id="box-9"></div>
</div>
```
# Table Example

| Functions            | Description |
| -----------          | ----------- |
| `handleWin()`        | Handle win of either player |
| `checkGameStatus()`  | Check the status after each turn |