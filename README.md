# Atividade - Semáforo offline

## Comando da Atividade



**Parte 1: Montagem Física do Semáforo**
Você deve realizar a montagem física de um semáforo utilizando LEDs e resistores em uma protoboard. Os LEDs devem representar as cores vermelho, amarelo e verde, seguindo o esquema de um semáforo convencional.



Para completar esta etapa:
- Conecte corretamente os LEDs e resistores na protoboard conforme o esquema.
- Certifique-se de usar os resistores adequadamente para proteger os LEDs.
- Organize a disposição dos fios para garantir clareza e facilidade de visualização.



**Parte 2: Programação e Lógica do Semáforo**
Você deve programar o comportamento do semáforo para alternar entre as fases vermelho, amarelo e verde, seguindo a lógica abaixo:
- 6 segundos no vermelho
- 2 segundos no amarelo
- 2 segundos no verde
- +2 segundos no verde (simulando um tempo adicional para pedestres terminarem a travessia)
- 2 segundos no amarelo



Esse ciclo deve ser repetido continuamente em um loop.



Para completar essa etapa:
- Escreva o código para controlar as luzes do semáforo com a temporização exata descrita.
- Teste o código e certifique-se de que as fases estão funcionando corretamente, com as transições e tempos esperados.


**Parte 3: Avaliação de Pares**


Nesta atividade, todos os alunos serão ser avaliadores e avaliados.
Cada atividade deverá ser avaliada por pelo menos dois alunos segundo os critérios do barema.



⚠️ Entrega: Poste em seu repositório pessoal do GitHub uma foto da sua montagem física (protoboard com LEDs conectados) e um breve relato explicando como foi feita a montagem e as conexões, adicione uma tabela com as especificações dos componentes utilizados. Adicione também em seu repositório do GitHub o código da programação do semáforo e um vídeo demonstrando o funcionamento do semáforo (LEDs acendendo e apagando conforme o código). Certifique-se de que o tempo de cada fase está correto. Adicionalmente, você também deve postar os resultados dos testes com os nomes completos dos avaliadores.

## Desenvolvimento da Atividade

&emsp; Para acesso ao vídeo do projeto da atividade ponderada, clique [aqui](https://drive.google.com/file/d/1Jn9CSACxZLLrNhRsXC_JtaTT2uk0eU9x/view?usp=sharing).

<div align="center">
  <sub>Figura 1 - Circuito de Semáforo montado</sub>
  
  <br>
  <img src="assets\montagem.jpg" alt="Montagem do Circuito de Semáforo"> <br>

</div>

## Código do Semáforo

```ino
#include <Wire.h>   
#include <LiquidCrystal_I2C.h>  

#define col 16   
#define lin 2      
#define ende 0x27 

// Define variáveis com a pinagem especificada
const int green = 7;
const int yellow = 6;
const int red = 5;
const int ped = 4;

LiquidCrystal_I2C lcd(ende, col, lin);  

// Setup do modo dos pinos e inicia parâmetros do LCD
void setup() {
  pinMode(green, OUTPUT);
  pinMode(yellow, OUTPUT);
  pinMode(red, OUTPUT);
  pinMode(ped, OUTPUT);
  lcd.init();      
  lcd.backlight(); 
  lcd.clear();  
}

// Função lcd_red para printar mensagem no LCD e atualizar o temporizador quando ao tempo restante do LED ligado
void lcd_red(int max) {  
  lcd.print("PEDESTRE AGUARDE!"); 
  lcd.setCursor(1, 0);     
  for (int sec_max = max / 1000; sec_max >= 0; sec_max--) {
    lcd.setCursor(1, 1);               
    lcd.print(sec_max + String(" SEGUNDOS RESTANTES")); 
    delay(1000);  
  }
}

// Função lcd_green para printar mensagem no LCD e atualizar o temporizador quando ao tempo restante do LED ligado
void lcd_green(int max) {
  lcd.print("PASSAGEM LIVRE!"); 
  lcd.setCursor(1, 0); 
  for (int sec_max = max / 1000; sec_max >= 0; sec_max--) {
    lcd.setCursor(1, 1);            
    lcd.print(sec_max + String(" SEGUNDOS RESTANTES"));  
    delay(1000);  
  }
}


void loop() {

  // Liga LED vermelho por 6 segundos e roda função lcd_red
  lcd.clear();
  digitalWrite(green, LOW);
  digitalWrite(yellow, LOW);
  digitalWrite(red, HIGH);
  digitalWrite(ped, LOW);
  lcd.setCursor(1, 0);            
  lcd_red(6000);         
  lcd.clear();

  // Imprime mensagem no LCD, ativa o LED amarelo por 2 segundos e pisca o LED de pedestres
  lcd.print("CUIDADO!"); 
  digitalWrite(green, LOW);
  digitalWrite(yellow, HIGH);
  digitalWrite(red, LOW);
  ped_blink();

  // Liga o LED verde por 4 segundos (2 segundos nomimais + 2 extras) e roda função lcd_green
  digitalWrite(green, HIGH);
  digitalWrite(yellow, LOW);
  digitalWrite(red, LOW);
  digitalWrite(ped, HIGH);
  lcd_green(4000);
  lcd.clear();

  // Imprime mensagem no LCD, ativa o LED amarelo por 2 segundos e pisca o LED de pedestres
  lcd.print("CUIDADO!"); 
  digitalWrite(green, LOW);
  digitalWrite(yellow, HIGH);
  digitalWrite(red, LOW);
  ped_blink();
}

// Função de blink do led de pedestres
void ped_blink() {
  digitalWrite(ped, HIGH);
  delay(250);
  digitalWrite(ped, LOW);
  delay(250);
  digitalWrite(ped, HIGH);
  delay(250);
  digitalWrite(ped, LOW);
  delay(250);
  digitalWrite(ped, HIGH);
  delay(250);
  digitalWrite(ped, LOW);
  delay(250);
  digitalWrite(ped, HIGH);
  delay(250);
  digitalWrite(ped, LOW);
  delay(250);
  digitalWrite(ped, HIGH);
}
```

## Relatório de conclusão

&emsp; Para montagem do semáforo, um circuito de LED simples foi instalado em uma protoboard, sendo controlado por um Arduino e exibindo informações em um LCD com módulo I2C embutido. A tabela 1 contempla os componentes utilizados na atividade.

<p align="center"> Tabela 1 - Componentes da atividade </p>

| Componente  | Quantidade | Ligação Eletrônica |
|----------------------|--------------------|------------------|
| LED Verde               | 1             | Resistor de 150 ohms e GDD         |
| LED Amarelo                 | 1             | Resistor de 150 ohms e GDD         |
| LED Vermelho                 | 1             | Resistor de 150 ohms e GDD         |
| LED Azul                 | 1             | Resistor de 150 ohms e GDD         |
| Arduino Uno | 1             | USB Externo         |
| Resistor 150 Ohms             | 4             | Pino positivo dos LEDs do Semáforo e pinos de alimentação dos LEDs          |
| Display LCD 16x2 + Módulo I2C                 | 1             | VCC (5V), GDD e pinos A05 e A04          | 

&emsp; O programa controla um semáforo com um display LCD, simulando as operações de um sistema de trânsito, utilizando LEDs para representar as luzes do semáforo (verde, amarelo e vermelho) e um LED adicional para um semáforo de pedestres. As mensagens de estado e o tempo restante são exibidos no LCD. O ciclo de funcionamento alterna entre:

- LED Vermelho aceso: Exibe "PEDESTRE AGUARDE!" por 6 segundos.
- LED Amarelo aceso: Exibe "CUIDADO!" por 2 segundos, piscando o LED de pedestres.
- LED Verde aceso: Exibe "PASSAGEM LIVRE!" por 4 segundos, permitindo a passagem.

# Avaliação Pares

### Avaliador: Leonardo Casal

| Critério                                                                                                 | Contempla (Pontos) | Contempla Parcialmente (Pontos) | Não Contempla (Pontos) | Observações do Avaliador |
|---------------------------------------------------------------------------------------------------------|--------------------|----------------------------------|--------------------------|---------------------------|
| Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores                | Até 3              | Até 1,5                            | 0                        |       Sistema contêm todos os componentes inclusive um LCD e uma LED azul para pedestres                    |
| Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo                  | Até 3              | Até 1,5                          | 0                        | O LCD possui uma lógica de contagem dos LEDs acessos                          |
| Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) | Até 3              | Até 1,5                          | 0                        |          Código implementa o que foi pedido com a adição de uma lógica para controlar a LCD                 |
| Extra: Implmeentou um componente de liga/desliga no semáforo e/ou usou ponteiros no código | Até 1              |  Até 0,5                         | 0                        |                       Utilizou LCD para mostrar o 'status' do semáforo    |
|  |                                                             |  | |**Pontuação Total**: 10|