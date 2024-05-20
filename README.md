<!DOCTYPE html>
<html lang="en">
<head>
    <title>Debjyoti Sarker's Website</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            background: url('https://i.pinimg.com/originals/a2/3f/03/a23f03f4cc66d800bd3b6dbc2c883dc0.jpg') no-repeat center center fixed;
            background-size: cover;
        }
        header {
            color: white;
            text-align: center;
        }
        header h1 {
            font-size: 3em; /* Increased font size */
        }
        main {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            padding: 1em;
            padding-bottom: 6em; /* Add padding to ensure space above footer */
            min-height: calc(100vh - 150px); /* Ensure the main content reaches the top of the footer */
        }
        .column {
            background-color: rgba(34, 34, 34, 0.8); /* Translucent dark gray */
            border: 1px solid #ccc;
            margin: 1em;
            padding: 1em;
            margin-bottom: 2em; /* Space between column and footer */
            width: calc(33.333% - 2em);
            box-sizing: border-box;
            text-align: center;
        }
        .column h2 {
            margin-top: 0;
            color: white; /* Ensure text is visible against dark background */
        }
        .p5-wrapper {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 600px; /* Ensure enough space for the canvas */
            width: 100%;
        }
        footer {
            color: white;
            text-align: center;
            padding: 1em 0;
            position: fixed;
            width: 100%;
            bottom: 0;
            background-color: rgba(51, 51, 51, 0.8); /* Gray background */
        }
        footer a {
            color: blue;
            font-weight: bold;
            text-decoration: none;
        }
        footer a:hover {
            text-decoration: underline;
        }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js"></script>
    <script>
let moveCount = 0;
let decisionArray = [];
let tempArray = [];

for (let i = 0; i < 3; i++) {
  tempArray[i] = [];
  decisionArray[i] = [];
}

function setup() {
  let canvas = createCanvas(360, 580); // Adjusted size for a vertical rectangle to fit almost the entire widget
  canvas.parent('p5-sketch');
  background("rgba(34, 34, 34, 0.1)");
  textSize(12);
  stroke("white");
  for (let i = 1; i < 3; i++) {
    line(0, i * height / 3, width, i * height / 3); // Horizontal lines
    line(i * width / 3, 0, i * width / 3, height); // Vertical lines
  }
  initArray(decisionArray);
  initArray(tempArray);
}

function initArray(x) {
  for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
      x[i][j] = "0";
    }
  }
}

function drawX(n) {
  let x = width / 6 + (width / 3) * ((n - 1) % 3);
  let y = height / 6 + (height / 3) * floor((n - 1) / 3);
  line(x - 30, y - 30, x + 30, y + 30); // Adjusted size of X
  line(x - 30, y + 30, x + 30, y - 30); // Adjusted size of X
}

function drawCircle(n) {
  fill("white");
  circle(
    width / 6 + (width / 3) * ((n - 1) % 3),
    height / 6 + (height / 3) * floor((n - 1) / 3),
    80 // Adjusted size of O
  );
}

function winConfirmed(n) {
  return (
    (n[0][0] == n[1][0] && n[0][0] == n[2][0] && n[0][0] != "0") ||
    (n[0][1] == n[1][1] && n[0][1] == n[2][1] && n[0][1] != "0") ||
    (n[0][2] == n[1][2] && n[0][2] == n[2][2] && n[0][2] != "0") ||
    (n[0][0] == n[0][1] && n[0][0] == n[0][2] && n[0][0] != "0") ||
    (n[1][0] == n[1][1] && n[1][0] == n[1][2] && n[1][0] != "0") ||
    (n[2][0] == n[2][1] && n[2][0] == n[2][2] && n[2][0] != "0") ||
    (n[0][0] == n[1][1] && n[0][0] == n[2][2] && n[0][0] != "0") ||
    (n[0][2] == n[1][1] && n[0][2] == n[2][0] && n[0][2] != "0")
  );
}

function computerRandomMove(grid1, grid2) {
  for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
      grid2[i][j] = grid1[i][j];
    }
  }

  for (let k = 1; k <= 9; k++) {
    let row = floor((k - 1) / 3);
    let col = (k - 1) % 3;

    if (grid2[row][col] == "0") {
      grid2[row][col] = "O";

      if (winConfirmed(grid2)) {
        grid1[row][col] = "O";
        return k;
      }

      grid2[row][col] = "0";
    }
  }

  for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
      grid2[i][j] = grid1[i][j];
    }
  }

  for (let k = 1; k <= 9; k++) {
    let row = floor((k - 1) / 3);
    let col = (k - 1) % 3;

    if (grid2[row][col] == "0") {
      grid2[row][col] = "X";
      if (winConfirmed(grid2)) {
        grid1[row][col] = "O";
        return k;
      }

      grid2[row][col] = "0";
    }
  }

  let looking = true;
  while (looking) {
    let squareNumber = round(random(1, 9));
    let row = floor((squareNumber - 1) / 3);
    let col = (squareNumber - 1) % 3;

    if (grid1[row][col] == "0") {
      looking = false;
      grid1[row][col] = "O";
      return squareNumber;
    }
  }
}

function consoleOutput(x) {
  for (let i = 0; i < 3; i++) {
    print(i + ": " + x[i][0] + " " + x[i][1] + " " + x[i][2]);
  }
}

function outputMove() {
  if (
    mouseX > 0 &&
    mouseX < width / 3 &&
    mouseY > 0 &&
    mouseY < height / 3 &&
    mouseIsPressed &&
    decisionArray[0][0] == "0"
  ) {
    drawX(1);
    moveCount++;
    decisionArray[0][0] = "X";
    consoleOutput(decisionArray);
  } else if (
    mouseX > width / 3 &&
    mouseX < (width / 3) * 2 &&
    mouseY > 0 &&
    mouseY < height / 3 &&
    mouseIsPressed &&
    decisionArray[0][1] == "0"
  ) {
    drawX(2);
    moveCount++;
    decisionArray[0][1] = "X";
    consoleOutput(decisionArray);
  }
  if (
    mouseX > (width / 3) * 2 &&
    mouseX < width &&
    mouseY > 0 &&
    mouseY < height / 3 &&
    mouseIsPressed &&
    decisionArray[0][2] == "0"
  ) {
    drawX(3);
    moveCount++;
    decisionArray[0][2] = "X";
    consoleOutput(decisionArray);
  }

  if (
    mouseX > 0 &&
    mouseX < width / 3 &&
    mouseY > height / 3 &&
    mouseY < (height / 3) * 2 &&
    mouseIsPressed &&
    decisionArray[1][0] == "0"
  ) {
    drawX(4);
    moveCount++;
    decisionArray[1][0] = "X";
    consoleOutput(decisionArray);
  }

  if (
    mouseX > width / 3 &&
    mouseX < (width / 3) * 2 &&
    mouseY > height / 3 &&
    mouseY < (height / 3) * 2 &&
    mouseIsPressed &&
    decisionArray[1][1] == "0"
  ) {
    drawX(5);
    moveCount++;
    decisionArray[1][1] = "X";
    consoleOutput(decisionArray);
  }

  if (
    mouseX > (width / 3) * 2 &&
    mouseX < width &&
    mouseY > height / 3 &&
    mouseY < (height / 3) * 2 &&
    mouseIsPressed &&
    decisionArray[1][2] == "0"
  ) {
    drawX(6);
    moveCount++;
    decisionArray[1][2] = "X";
    consoleOutput(decisionArray);
  }

  if (
    mouseX > 0 &&
    mouseX < width / 3 &&
    mouseY > (height / 3) * 2 &&
    mouseY < height &&
    mouseIsPressed &&
    decisionArray[2][0] == "0"
  ) {
    drawX(7);
    moveCount++;
    decisionArray[2][0] = "X";
    consoleOutput(decisionArray);
  }

  if (
    mouseX > width / 3 &&
    mouseX < (width / 3) * 2 &&
    mouseY > (height / 3) * 2 &&
    mouseY < height &&
    mouseIsPressed &&
    decisionArray[2][1] == "0"
  ) {
    drawX(8);
    moveCount++;
    decisionArray[2][1] = "X";
    consoleOutput(decisionArray);
  }
  if (
    mouseX > (width / 3) * 2 &&
    mouseX < width &&
    mouseY > (height / 3) * 2 &&
    mouseY < height &&
    mouseIsPressed &&
    decisionArray[2][2] == "0"
  ) {
    drawX(9);
    moveCount++;
    decisionArray[2][2] = "X";
    consoleOutput(decisionArray);
  }
}

function checkWin(n) {
  if (winConfirmed(n)) {
    if (moveCount % 2 == 1) {
      noLoop();
      background("rgba(34, 34, 34, 0.3)");
      textSize(20);
      fill("white");
      text("X won", width / 2 - 20, height / 2);
    } else {
      noLoop();
      background("rgba(34, 34, 34, 0.3)");
      textSize(20);
      fill("white");
      text("O won", width / 2 - 20, height / 2);
    }
  } else {
    if (moveCount == 9) {
      noLoop();
      background("rgba(34, 34, 34, 0.3)");
      textSize(20);
      fill("white");
      text("Tie", width / 2 - 20, height / 2);
    }
  }
}

function draw() {
  if (moveCount % 2 == 0) {
    outputMove();
  } else {
    drawCircle(computerRandomMove(decisionArray, tempArray));
    moveCount++;
    consoleOutput(decisionArray);
  }

  checkWin(decisionArray);
}

        function updateDateTime() {
            const now = new Date();
            document.getElementById('datetime').textContent = now.toString();
        }
        setInterval(updateDateTime, 1000);

        function showPage(pageId) {
            document.querySelectorAll('.page').forEach(page => {
                page.style.display = 'none';
            });
            document.getElementById(pageId).style.display = 'block';
        }
    </script>
    <!-- Google Analytics -->
<body onload="showPage('home-page')">
    <header>
        <h1>Debjyoti Sarker's Website</h1>
    </header>

    <!-- Home Page Content -->
    <div id="home-page" class="page">
        <main>
            <div class="column">
                <h2>Widget 1: Current Date and Time</h2>
                <div id="datetime"></div>
            </div>
            <div class="column">
                <h2>Google Analytics</h2>
                <p>Google Analytics is active on this page.</p>
            </div>
            <div class="column">
                <h2>Tic Tac Toe (Not Impossible)</h2>
                <div class="p5-wrapper">
                    <div id="p5-sketch"></div>
                </div>
            </div>
        </main>
        <footer>
            <p>did i cook</p>
            <p><a href="#" onclick="showPage('another-page')">Go to Another Page</a></p>
        </footer>
    </div>

    <!-- Another Page Content -->
    <div id="another-page" class="page" style="display: none;">
        <header>
            <h1>Another Page</h1>
        </header>
        <main>
            <h2>This is another page on the website.</h2>
            <p>Here you can add more content as needed.</p>
        </main>
        <footer>
            <p>&copy; 2024 Debjyoti Sarker's Website</p>
            <p><a href="#" onclick="showPage('home-page')">Go to Home Page</a></p>
        </footer>
    </div>
</body>
</html>
