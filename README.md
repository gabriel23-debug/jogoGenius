# jogoGenius
Jogo Genius com arduino.

//Curso do professor José de Assis;  https://youtu.be/VsCZ-D6qbRc

/*
   Arduino jogo GENIUS
   @autor MISTER COSTELA
   curso do professor José de Assis; https://youtu.be/VsCZ-D6qbRc
*/
/*variaveis globais*/

int sequencia[32] = {};
int botoes[4] = {8, 9, 10, 11};
int leds[4] = {2, 3, 4, 5};
int tons[4] = {262, 294, 330, 349};
int rodada = 0;
int passo = 0;
int botaoPressionado = 0;
bool gameOver = false;


void setup() {
  //Leds
  pinMode(2, OUTPUT);
  pinMode(3, OUTPUT);
  pinMode(4, OUTPUT);
  pinMode(5, OUTPUT);
  //Buzzer
  pinMode(7, OUTPUT);
  //Botões
  pinMode(8, INPUT);
  pinMode(9, INPUT);
  pinMode(10, INPUT);
  pinMode(11, INPUT);
  Serial.begin(9600);
}

void loop() {
  proximaRodada();
  reproduzirSequencia();
  aguardarJogador();
  //reiniciar todas as variaveis
  if (gameOver == true) {
    sequencia[32] = {};
    rodada = 0;
    passo = 0;
    gameOver = false;
  }
  delay(1000);
}

//FUNÇÕES

void proximaRodada() {
  int sorteio = random (4);
  sequencia [rodada] = sorteio;
  rodada = rodada + 1;
  //Serial.print(sorteio);
}

void reproduzirSequencia() {
  for (int i = 0; i < rodada; i++) {
    tone (7, tons[sequencia[i]]);
    digitalWrite(leds[sequencia[i]], HIGH);
    delay(500);
    noTone(7);
    digitalWrite(leds[sequencia[i]], LOW);
    delay(100);
  }
}

void aguardarJogador() {
  for (int i = 0; i < rodada; i++) {
    bool jogadaEfetuada = false;
    while (!jogadaEfetuada) {
      for (int i = 0; i <= 3; i++) {
        if (digitalRead (botoes[i]) == HIGH) {
          botaoPressionado = i;
          tone (7, tons[i]);
          digitalWrite (leds[i], HIGH);
          delay(300);
          digitalWrite (leds[i], LOW);
          noTone(7);
          jogadaEfetuada = true;
        }
      }
    }
    //verificar a jogada
    if (sequencia [passo] != botaoPressionado) {
      for (int i = 0; i <= 0; i++) {
        tone(7, 70);
        digitalWrite(leds[i], HIGH);
        delay(100);
        digitalWrite(leds[i], LOW);
        noTone(7);
      }
      gameOver = true;
      break;
    }
    passo = passo + 1;    
  }
  passo = 0;
}
