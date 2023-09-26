# PROJETO VINHERIA AGNELLO
# Sobre o projeto

https://www.tinkercad.com/things/0GSlu9A5Gfq?sharecode=wUoY6CbAqYJSJGcPSbKxCSyOoa1ZTmpdIcmFgMGk7NQ

O sistema apresentado é uma parte integrante do projeto Vinheria Agnello, cujo propósito é o monitoramento dos estoques de vinho. Utilizando o Arduino e sensores, realiza-se a captura das informações de luminosidade e temperatura do ambiente. Com os dados coletados, o sistema interpreta e fornece notificações sobre a situação atual do ambiente, assegurando um maior controle sobre os estoques e evitando cenários desfavoráveis para o armazenamento dos produtos.

## Circuito montado no simulador
https://fiapcom-my.sharepoint.com/:i:/g/personal/rm552345_fiap_com_br/EU_LAj7C7bxNk-NmRuMFdesBHNE3YAxu4GGfvpsKWMrqkw?e=o4Wnk7

## Vídeo explicativo
https://fiapcom-my.sharepoint.com/:v:/g/personal/rm552345_fiap_com_br/EbEPF9eYILlEsF8u9MqKqkYBa5FquuhJ0xqiy6gSzK3W7Q?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0RpcmVjdCJ9fQ&e=gHLa2w

# Tecnologia utilizada
- Linguagem C

# Componentes utilizados
- 1 Arduino UNO
- 1 protoboard
- 3 leds (verde, amarelo e vermelho)
- 3 resistores de 220 Ohms
- 1 resistor de 10 Kilo Ohms
- 1 buzzer
- 1 LDR
- 1 TMP36
- 1 potenciômetro
- jumpers

# Código-fonte

```c
// Definindo o pino dos Leds
int ledVerde 	= 2;
int ledAmarelo 	= 3;
int ledVermelho = 4;

// Definindo o pino do buzzer
int buzzer 	= 5;

// Definindo o pino/variaveis do sensor de temp
int temperatura = A1;
int celsius = 0;

// Definindo o pino/variaveis do sensor de umidade
int pinUmidade = A2;
float voltagem;
float leitura;
int umidade;


void setup()
{
  // Inicia a comunicação serial
  Serial.begin(9600);
  
  // Definindo as entradas e saídas
  
    // buzzer
    pinMode(buzzer, OUTPUT);

    // Leds
    pinMode(ledVerde, OUTPUT);
    pinMode(ledAmarelo, OUTPUT);
    pinMode(ledVermelho, OUTPUT);

    // Sensor Temperatura
    pinMode(temperatura, INPUT);
  
  	// Sensor Umidade 
  	pinMode(pinUmidade, INPUT);
  
}

void loop()
{
  // Definindo o pino analógico do LDR
  int LDR = analogRead(A0);
  
  // Medida do sensor de temperatura em Celsius
  celsius = map(((analogRead(temperatura)- 20) * 3.04), 0, 1023, -40, 125);
  
  // Definindo a umidade
  	// Ler o pino do sensor de umidade
    leitura = analogRead(pinUmidade);
  	// Cálculo da voltagem
  	voltagem = leitura*1053/1023;
  	// Converter a voltagem em % de umidade
  	umidade = voltagem/10.53;
  
  // PROCESSANDO OS DADOS
  // Caso em que o ambiente é favorável
  if (LDR <= 800 && celsius >= 10 && celsius <= 16 && umidade >= 60 && umidade <= 80){
    
    Serial.println("---- FAVORAVEL ----");
    
    digitalWrite(ledVerde, HIGH);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, LOW);
    digitalWrite(buzzer, LOW);
    
  }
  
  // Aviso de mudança do ambiente
  if(LDR > 800 && LDR <= 900 || celsius >= 6 && celsius < 10 || celsius <= 19 && celsius > 16 || umidade < 60 && umidade > 50 || umidade > 80 && umidade < 90){
    
    Serial.println("**** ATENCAO ****");

    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, HIGH);
    digitalWrite(ledVermelho, LOW);
    digitalWrite(buzzer, LOW);
    
  }
  // Caso em que o ambiente NÃO é favorável
  if(LDR > 900 || celsius < 6 || celsius > 19 || umidade <= 50 || umidade >= 90){
    
    Serial.println("!!!! ALERTA !!!!");  
    
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, HIGH);
    digitalWrite(buzzer, HIGH);
    
    delay(3000);
    
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, HIGH);
    digitalWrite(buzzer, LOW);
    
  }
  
   // PREVIEW OS DADOS
      
  	Serial.println("    ");
    Serial.print("Luminosidade: ");
    Serial.println(LDR);
    
    Serial.print("Temperatura: ");
    Serial.print(celsius);
    Serial.println(" c ");
    
    Serial.print("Umidade: ");
    Serial.print(umidade);
    Serial.println(" % ");
    
    Serial.println("    ");

  
  delay(1000);
}
```

# Autor

Gilson Dias Ramos Junior

www.linkedin.com/in/gilson-dias-jn
