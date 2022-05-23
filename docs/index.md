<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<title>Snake Game</title>
<style type="text/css">
body {text-align:center;}
canvas { border:7px dashed #4A4747 }
h1 { font-size:35px; text-align: center; margin: 0; padding-bottom: 25px; text-decoration: underline; font-family: Geneva; color: #0520A5;}
</style>
<script type="text/javascript">
function play_game()
{
var level = 160; // Game level, by decreasing will speed up
var rect_w = 45; // Width
var rect_h = 30; // Height
var inc_score = 50; // Score
var snake_color = "#0520A5"; // Snake Color
var ctx; // Canvas attributes
var tn = []; // temp directions storage
var x_dir = [-1, 0, 1, 0]; // position adjusments
var y_dir = [0, -1, 0, 1]; // position adjusments
var queue = [];
var frog = 1; // defalut food
var map = [];
var MR = Math.random;
var X = 5 + (MR() * (rect_w - 10))|0; // Calculate positions
var Y = 5 + (MR() * (rect_h - 10))|0; // Calculate positions
var direction = MR() * 3 | 0;
var interval = 0;
var score = 0;
var sum = 0, easy = 0;
var i, dir;
// getting play area 
var c = document.getElementById('playArea');
ctx = c.getContext('2d');
// Map positions
for (i = 0; i < rect_w; i++)
{
map[i] = [];
}
// random placement of snake food
function random_snake()
{
var x, y;
do
{
x = MR() * rect_w|0;
y = MR() * rect_h|0;
}
while (map[x][y]);
map[x][y] = 1;
ctx.fillStyle = snake_color;
ctx.strokeRect(x * 10+1, y * 10+1, 8, 8);
}
// Default somewhere placement
random_snake();
function set_game_speed()
{
if (easy)
{
X = (X+rect_w)%rect_w;
Y = (Y+rect_h)%rect_h;
}
--inc_score;
if (tn.length)
{
dir = tn.pop();
if ((dir % 2) !== (direction % 2))
{
direction = dir;
}
}
if ((easy || (0 <= X && 0 <= Y && X < rect_w && Y < rect_h)) && 2 !== map[X][Y])
{
if (1 === map[X][Y])
{
score+= Math.max(5, inc_score);
inc_score = 50;
random_snake();
frog++;
}
//ctx.fillStyle("#ffffff");
ctx.fillRect(X * 10, Y * 10, 9, 9);
map[X][Y] = 2;
queue.unshift([X, Y]);
X+= x_dir[direction];
Y+= y_dir[direction];
if (frog < queue.length)
{
dir = queue.pop()
map[dir[0]][dir[1]] = 0;
ctx.clearRect(dir[0] * 10, dir[1] * 10, 10, 10);
}
}
else if (!tn.length)
{
var show_score = document.getElementById("show");
show_score.innerHTML = "You lose!<br /> <u>Your Score:</u> <b>"+score+"</b><br><br> Want to try again?<br><br><input type='button' value='Play Again' onclick='window.location.reload();' />";
document.getElementById("playArea").style.display = 'none';
window.clearInterval(interval);
}
}
interval = window.setInterval(set_game_speed, level);
document.onkeydown = function(e) {
var code = e.keyCode - 37;
if (0 <= code && code < 4 && code !== tn[0])
{
tn.unshift(code);
}
else if (-5 == code)
{
if (interval)
{
window.clearInterval(interval);
interval = 0;
}
else
{
interval = window.setInterval(set_game_speed, 60);
}
}
else
{
dir = sum + code;
if (dir == 44||dir==94||dir==126||dir==171) {
sum+= code
} else if (dir === 218) easy = 1;
}
}
}
</script>
</head>
<body onload="play_game()">
<h1>Play Snake Game</h1>
<div id="show"></div>
<canvas id="playArea" width="450" height="300">Sorry your browser does not support HTML5. Try using like Chrome or Firefox.</canvas>
</body>
</html>

<!DOCTYPE html>
<html>
<head>
<meta name="viaewport" content="width=device-width, initial-scale=1.0"/>
<style>
canvas {
    border:15px solid #D6EA0D;
    background-color: #050505;
}
</style>
</head>
<body onload="startGame()">
<script>

var myGamePiece;
var myObstacles = [];
var myScore;

function startGame() {
    myGamePiece = new component(30, 30, "white", 10, 120);
    myScore = new component("30px", "Consolas", "yellow", 280, 40, "text");
    myGameArea.start();
}

var myGameArea = {
    canvas : document.createElement("canvas"),
    start : function() {
        this.canvas.width = 480;
        this.canvas.height = 270;
        this.context = this.canvas.getContext("2d");
        document.body.insertBefore(this.canvas, document.body.childNodes[0]);
        this.frameNo = 0;
        this.interval = setInterval(updateGameArea, 15);
        },
    clear : function() {
        this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
    },
    stop : function() {
        clearInterval(this.interval);
    }
}

function component(width, height, color, x, y, type) {
    this.type = type;
    this.width = width;
    this.height = height;
    this.speedX = 0;
    this.speedY = 0;    
    this.x = x;
    this.y = y;    
    this.update = function() {
        ctx = myGameArea.context;
        if (this.type == "text") {
            ctx.font = this.width + " " + this.height;
            ctx.fillStyle = color;
            ctx.fillText(this.text, this.x, this.y);
        } else {
            ctx.fillStyle = color;
            ctx.fillRect(this.x, this.y, this.width, this.height);
        }
    }
    this.newPos = function() {
        this.x += this.speedX;
        this.y += this.speedY;        
    }
    this.crashWith = function(otherobj) {
        var myleft = this.x;
        var myright = this.x + (this.width);
        var mytop = this.y;
        var mybottom = this.y + (this.height);
        var otherleft = otherobj.x;
        var otherright = otherobj.x + (otherobj.width);
        var othertop = otherobj.y;
        var otherbottom = otherobj.y + (otherobj.height);
        var crash = true;
        if ((mybottom < othertop) || (mytop > otherbottom) || (myright < otherleft) || (myleft > otherright)) {
            crash = false;
        }
        return crash;
    }
}

function updateGameArea() {
    var x, height, gap, minHeight, maxHeight, minGap, maxGap;
    for (i = 0; i < myObstacles.length; i += 1) {
        if (myGamePiece.crashWith(myObstacles[i])) {
            myGameArea.stop();
            return;
        } 
    }
    myGameArea.clear();
    myGameArea.frameNo += 1;
    if (myGameArea.frameNo == 1 || everyinterval(150)) {
        x = myGameArea.canvas.width;
        minHeight = 20;
        maxHeight = 200;
        height = Math.floor(Math.random()*(maxHeight-minHeight+1)+minHeight);
        minGap = 50;
        maxGap = 200;
        gap = Math.floor(Math.random()*(maxGap-minGap+1)+minGap);
        myObstacles.push(new component(10, height, "green", x, 0));
        myObstacles.push(new component(10, x - height - gap, "green", x, height + gap));
    }
    for (i = 0; i < myObstacles.length; i += 1) {
        myObstacles[i].speedX = -1;
        myObstacles[i].newPos();
        myObstacles[i].update();
    }
    myScore.text="SCORE: " + myGameArea.frameNo;
    myScore.update();
    myGamePiece.newPos();    
    myGamePiece.update();
}

function everyinterval(n) {
    if ((myGameArea.frameNo / n) % 1 == 0) {return true;}
    return false;
}

function moveup() {
    myGamePiece.speedY = -1; 
}

function movedown() {
    myGamePiece.speedY = 1; 
}

function moveleft() {
    myGamePiece.speedX = -1; 
}

function moveright() {
    myGamePiece.speedX = 1; 
}

function clearmove() {
    myGamePiece.speedX = 0; 
    myGamePiece.speedY = 0; 
}
</script>
<div style="text-align:center;width:480px;">
  <button onmousedown="moveup()" onmouseup="clearmove()" ontouchstart="moveup()">UP</button><br><br>
  <button onmousedown="moveleft()" onmouseup="clearmove()" ontouchstart="moveleft()">LEFT</button>
  <button onmousedown="moveright()" onmouseup="clearmove()" ontouchstart="moveright()">RIGHT</button><br><br>
  <button onmousedown="movedown()" onmouseup="clearmove()" ontouchstart="movedown()">DOWN</button>
</div>

<p>The score will count one point for each frame you manage to "stay alive".</p>
</body>
</html>
