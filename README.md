# boardmatejs

_boardmatejs_ is a free/libre open source chess UI developed for
using the best.
It targets modern browsers, as well as mobile development using Cordova.
It is also a TypeScript chess library used for chess move generation/validation, piece placement/movement, and check/checkmate/stalemate detection - basically everything.You can code the ai to it because it is also a JavaScript chessboard where you can add an ai.
## License

boardmatejs is distributed under the **GPL-3.0 license** (or any later version,
at your option).
When you use Chessground for your website, your combined work may be
distributed only under the GPL. **You must release your source code** to the
users of your website.

Please read more about GPL for JavaScript on [greendrake.info](https://greendrake.info/publications/js-gpl).



## Features

boardmatejs is designed to fulfill all lichess.org web and mobile apps needs, so it is pretty featureful.

- Well typed with TypeScript
- Fast. Uses a custom DOM diff algorithm to reduce DOM writes to the absolute minimum.
- Small footprint: 10K gzipped (31K unzipped). No dependencies.
- SVG drawing of circles, arrows, and custom user shapes on the board
- Arrows snap to valid moves. Freehand arrows can be drawn by dragging the mouse off the board and back while drawing an arrow.
- Entirely configurable and reconfigurable at any time
- Styling with CSS only: board and pieces can be changed by simply switching a class
- Fluid layout: board can be resized at any time
- Support for 3D pieces and boards
- Full mobile support (touchstart, touchmove, touchend)
- Move pieces by click
- Move pieces by drag & drop
  - Minimum distance before drag
  - Centralisation of the piece under the cursor
  - Piece ghost element
  - Drop off revert or trash
- Premove by click or drag
- Drag new pieces onto the board (editor, Crazyhouse)
- Animation of pieces: moving and fading away
- Display last move, check, move destinations, and premove destinations (hover effects possible)
- Import and export positions in FEN notation
- User callbacks
- No chess logic inside: can be used for chess variants
- etc.
## Installation

```sh
npm install chess.js @chrisoakman/chessboardjs @chrisoakman/chessboard2 @lichess-org/chessground jquery
```

### Usage
```html
<!DOCTYPE html>
<html>
<head>
    <title>Chess Libs Setup</title>
    <link rel="stylesheet" href="node_modules/@lichess-org/chessground/assets/chessground.base.css">
    <link rel="stylesheet" href="node_modules/@lichess-org/chessground/assets/chessground.brown.css">
    <link rel="stylesheet" href="node_modules/@chrisoakman/chessboardjs/dist/css/chessboard-1.0.0.min.css">
    <link rel="stylesheet" href="node_modules/@chrisoakman/chessboard2/dist/chessboard2.min.css">
</head>
<body>
    <div id="board1" style="width: 400px"></div>
    <div id="board2" style="width: 400px"></div>
    <script src="bundle.js"></script> <!-- Your compiled JS -->
</body>
</html>

```
```js
// Import libraries
import { Chess } from 'chess.js';
import $ from 'jquery';
import { Chessboard } from '@chrisoakman/chessboardjs';
import { Chessground } from '@lichess-org/chessground';


const game = new Chess();


var board1 = Chessboard('board1', {
    draggable: true,
    position: 'start',
    onDrop: handleMove
});

function handleMove(source, target) {
    let move = game.move({ from: source, to: target, promotion: 'q' });
    if (move === null) return 'snapback'; // Invalid move
    board1.position(game.fen()); // Update board
    
    // Also update the chessground board
    ground.set({ fen: game.fen() });
}

const ground = Chessground(document.getElementById('board2'), {
    fen: game.fen(),
    movable: {
        color: 'white',
        dests: toDests(game)
    }
});

// Helper to convert chess.js moves to chessground format
function toDests(game) {
    const dests = {};
    game.SQUARES.forEach(s => {
        const ms = game.moves({square: s, verbose: true});
        if (ms.length) dests[s] = ms.map(m => m.to);
    });
    return dests;
}

```
More? Please make a pull request to include it here.



The release workflow will increment the package.json version, create the tag, the github release, and publish to npm
