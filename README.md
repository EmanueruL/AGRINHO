function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220);
}
// Definindo variáveis globais
let jardineiro;
let plantas = [];
let temperatura = 15.9;
let totalArvores = 0;
let jogoAtivo = true;

// Classe para o personagem jardineiro
class Jardineiro {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.velocidade = 5;
    this.tamanho = 30;
  }

  // Método para atualizar a posição do jardineiro
  atualizar() {
    if (keyIsDown(LEFT_ARROW)) {
      this.x -= this.velocidade;
    }
    if (keyIsDown(RIGHT_ARROW)) {
      this.x += this.velocidade;
    }
    if (keyIsDown(UP_ARROW)) {
      this.y -= this.velocidade;
    }
    if (keyIsDown(DOWN_ARROW)) {
      this.y += this.velocidade;
    }

    // Limitar movimento dentro da tela
    this.x = constrain(this.x, 0, width);
    this.y = constrain(this.y, 0, height);
  }

  // Método para desenhar o jardineiro
  mostrar() {
    fill(255, 0, 0);
    ellipse(this.x, this.y, this.tamanho);
  }
}

// Classe para as árvores
class Arvore {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.tamanho = 40;
    this.crescimento = 0;
  }

  // Método para desenhar a árvore
  mostrar() {
    // Tronco
    fill(139, 69, 19);
    rect(this.x - 5, this.y - 20, 10, 20);

    // Copa da árvore (cresce com o tempo)
    fill(34, 139, 34);
    ellipse(
      this.x,
      this.y - 30 - this.crescimento,
      this.tamanho + this.crescimento
    );

    // Animação de crescimento
    if (this.crescimento < 10) {
      this.crescimento += 0.1;
    }
  }
}

function setup() {
  createCanvas(800, 500);
  jardineiro = new Jardineiro(width / 2, height - 50);
  textFont("Arial");
}

function draw() {
  // Fundo gradiente baseado na temperatura
  let corFundo = lerpColor(
    color(255, 153, 0), // (quente)
    color(153, 255, 204), // (frio)
    map(temperatura, 10, 50, 0, 1)
  );
  background(corFundo);

  // Mostrar informações e instruções
  mostrarInformacoes();

  // Atualizar e mostrar o jardineiro
  jardineiro.atualizar();
  jardineiro.mostrar();

  // Mostrar todas as árvores
  for (let arvore of plantas) {
    arvore.mostrar();
  }

  // Aumentar temperatura gradualmente (se o jogo está ativo)
  if (jogoAtivo) {
    temperatura += 0.01;

    // Verificar fim do jogo
    verificarFimDeJogo();
  }
}

// Função para mostrar as informações na tela
function mostrarInformacoes() {
  textSize(26);
  fill(0);
  text("Vamos plantar árvores para reduzir a temperatura?", 20, 30);

  textSize(16);
  text("Para movimentar o personagem use as setas do teclado.", 20, 60);
  text("Para plantar árvores use P ou Espaço.", 20, 85);

  // Mostrar status do jogo
  textSize(18);
  fill(255);
  text("Temperatura: " + temperatura.toFixed(2) + "°C", 20, height - 20);
  text("Árvores plantadas: " + totalArvores, width - 200, height - 20);
}

// Função para verificar se o jogo acabou
function verificarFimDeJogo() {
  if (temperatura >= 50) {
    mostrarMensagemDeDerrota();
    jogoAtivo = false;
  } else if (totalArvores >= 30) {
    mostrarMensagemDeVitoria();
    jogoAtivo = false;
  }
}

// Função para mostrar mensagem de vitória
function mostrarMensagemDeVitoria() {
  fill(0, 100);
  rect(0, 0, width, height);

  textSize(32);
  fill(255);
  textAlign(CENTER, CENTER);
  text("Você venceu!", width / 2, height / 2 - 30);
  text("Você plantou muitas árvores!", width / 2, height / 2 + 10);
  textSize(20);
  text("Pressione R para reiniciar", width / 2, height / 2 + 50);
  textAlign(LEFT);
}

// Função para mostrar mensagem de derrota
function mostrarMensagemDeDerrota() {
  fill(0, 100);
  rect(0, 0, width, height);

  textSize(32);
  fill(255);
  textAlign(CENTER, CENTER);
  text("Você perdeu!", width / 2, height / 2 - 30);
  text("A temperatura está muito alta!", width / 2, height / 2 + 10);
  textSize(20);
  text("Pressione R para reiniciar", width / 2, height / 2 + 50);
  textAlign(LEFT);
}

// Função para plantar árvores
function keyPressed() {
  if ((key === " " || key === "p" || key === "P") && jogoAtivo) {
    let arvore = new Arvore(jardineiro.x, jardineiro.y);
    plantas.push(arvore);
    totalArvores++;
    temperatura -= 0.5; // Reduz a temperatura ao plantar
    if (temperatura < 10) temperatura = 10;
  }

  // Reiniciar o jogo
  if (key === "r" || key === "R") {
    reiniciarJogo();
  }
}

// Função para reiniciar o jogo
function reiniciarJogo() {
  plantas = [];
  temperatura = 15.9;
  totalArvores = 0;
  jogoAtivo = true;
}
