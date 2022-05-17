Pfeffermühlen AG

Hallo hier ist Pfeffermühlenmann:)

Ich habe heute vorgehabt euch mit cuten Komplimenten zu beglücken :)


![Download (1)](https://user-images.githubusercontent.com/105705281/168764945-3da5092f-d38a-4fc3-81e3-570ac27ea2f7.jpg)


gans gans vriische wuagwuast gibts hio ich bin nicht in Schulhäuser erlaubt da ich 19 Straftaten begangen bin ich bin kurz vom sterben.


<img src="https://i.makeagif.com/media/5-13-2018/QNvZv0.gif" alt="funny GIF" width="100%">

function component(width, height, color, x, y) {
  this.gamearea = gamearea;
  this.width = width;
  this.height = height;
  this.angle = 0;
  this.speed = 1;
  this.x = x;
  this.y = y;
  this.update = function() {
    ctx = myGameArea.context;
    ctx.save();
    ctx.translate(this.x, this.y);
    ctx.rotate(this.angle);
    ctx.fillStyle = color;
    ctx.fillRect(this.width / -2, this.height / -2, this.width, this.height);
    ctx.restore();
  }
  this.newPos = function() {
    this.x += this.speed * Math.sin(this.angle);
    this.y -= this.speed * Math.cos(this.angle);
  }
}
