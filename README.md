let paddleWidth = 10;
let paddleHeight = 80;
let ballSize = 12;
let leftPaddle, rightPaddle, ball;
let leftScore = 0;
let rightScore = 0;

function setup() {
  createCanvas(800, 400);
  
  // Criação das raquetes e da bola
  leftPaddle = new Paddle(true);
  rightPaddle = new Paddle(false);
  ball = new Ball();
}

function draw() {
  background(0);
  
  // Atualiza e exibe as raquetes e a bola
  leftPaddle.update();
  rightPaddle.update();
  ball.update();
  
  leftPaddle.show();
  rightPaddle.show();
  ball.show();
  
  // Verifica colisões
  ball.checkPaddleCollision(leftPaddle);
  ball.checkPaddleCollision(rightPaddle);
  
  // Exibe o placar
  displayScore();
}

// Classe Paddle
class Paddle {
  constructor(isLeft) {
    this.isLeft = isLeft;
    this.y = height / 2 - paddleHeight / 2;
    this.x = this.isLeft ? 0 : width - paddleWidth;
  }
  
  update() {
    if (this.isLeft) {
      if (keyIsDown(87)) { // W key
        this.y -= 5;
      }
      if (keyIsDown(83)) { // S key
        this.y += 5;
      }
    } else {
      if (keyIsDown(UP_ARROW)) {
        this.y -= 5;
      }
      if (keyIsDown(DOWN_ARROW)) {
        this.y += 5;
      }
    }
    
    // Limitar movimento das raquetes
    this.y = constrain(this.y, 0, height - paddleHeight);
  }
  
  show() {
    fill(255);
    rect(this.x, this.y, paddleWidth, paddleHeight);
  }
}

// Classe Ball
class Ball {
  constructor() {
    this.reset();
  }
  
  reset() {
    this.x = width / 2;
    this.y = height / 2;
    this.xSpeed = random(5, 7) * (random() < 0.5 ? 1 : -1);
    this.ySpeed = random(-3, 3);
  }
  
  update() {
    this.x += this.xSpeed;
    this.y += this.ySpeed;
    
    // Verifica colisão com o teto e o chão
    if (this.y <= 0 || this.y >= height) {
      this.ySpeed *= -1;
    }
    
    // Verifica se a bola saiu da tela
    if (this.x < 0) {
      rightScore++;
      this.reset();
    } else if (this.x > width) {
      leftScore++;
      this.reset();
    }
  }
  
  checkPaddleCollision(paddle) {
    if (this.x <= paddle.x + paddleWidth && this.x >= paddle.x &&
        this.y + ballSize >= paddle.y && this.y <= paddle.y + paddleHeight) {
      this.xSpeed *= -1;
      this.x += this.xSpeed; // Move a bola para fora da raquete
    }
  }
  
  show() {
    fill(255);
    ellipse(this.x, this.y, ballSize);
  }
}

// Função para exibir o placar
function displayScore() {
  textSize(32);
  fill(255);
  text(leftScore, width / 4, 50);
  text(rightScore, (3 * width) / 4, 50);
}
