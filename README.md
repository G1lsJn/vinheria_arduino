# PROJETO VINHERIA AGNELLO
# Sobre o projeto

https://www.tinkercad.com/things/0GSlu9A5Gfq?sharecode=wUoY6CbAqYJSJGcPSbKxCSyOoa1ZTmpdIcmFgMGk7NQ

O sistema apresentado é uma parte integrante do projeto Vinheria Agnello, cujo propósito é o monitoramento dos estoques de vinho. Utilizando o Arduino e sensores, realiza-se a captura das informações de luminosidade e temperatura do ambiente. Com os dados coletados, o sistema interpreta e fornece notificações sobre a situação atual do ambiente, assegurando um maior controle sobre os estoques e evitando cenários desfavoráveis para o armazenamento dos produtos.

## Circuito montado no simulador
https://fiapcom-my.sharepoint.com/:i:/g/personal/rm552345_fiap_com_br/EXdYBxzdbx1Li_USYgu1EwgB4pb5j33FoABkMdv31qLNNg?e=XYVSwi

## Vídeo explicativo
- Link do vídeo

# Tecnologia utilizada
- Linguagem C

# Código-fonte

```c
// Definindo o pino dos Leds
int ledVerde 	= 2;
int ledAmarelo 	= 3;
int ledVermelho = 4;

// Definindo o pino do buzzer
int buzzer 	= 5;

// Definindo o pino do sensor de temp
int temperatura = A1;
int sensor = 0;
int celsius = 0;

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
  
}

void loop()
{
  // Definindo o pino analógico do LDR
  int LDR = analogRead(A0);
  
  // Medida do sensor de temperatura em Celsius
  sensor = analogRead(temperatura);
  celsius = map(((analogRead(temperatura)- 20) * 3.04), 0, 1023, -40, 125);
  
  // Caso em que o ambiente é favorável
  if (LDR <= 800 && celsius <= 13){
    
    Serial.println("---- FAVORAVEL ----");
    Serial.println("    ");
    
    Serial.print("Luminosidade: ");
    Serial.println(LDR);
    
    Serial.print("Temperatura: ");
    Serial.print(celsius);
    Serial.println(" c ");
    Serial.println("    ");
    
    digitalWrite(ledVerde, HIGH);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, LOW);
    digitalWrite(buzzer, LOW);
    
  }
  // Aviso de mudança do ambiente
  if(LDR > 800 && LDR <= 900 || celsius > 13 && celsius <= 16){
    
    Serial.println("**** ATENCAO ****");
    Serial.println("    ");
    
    Serial.print("Luminosidade: ");
    Serial.println(LDR);
    
    Serial.print("Temperatura: ");
    Serial.print(celsius);
    Serial.println(" c ");
    Serial.println("    ");
    
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, HIGH);
    digitalWrite(ledVermelho, LOW);
    digitalWrite(buzzer, LOW);
    
  }
  // Caso em que o ambiente NÃO é favorável
  if(LDR > 900 || celsius > 16){
    
    Serial.println("!!!! ALERTA !!!!");
    Serial.println("    ");
    
    Serial.print("Luminosidade: ");
    Serial.println(LDR);
    
    Serial.print("Temperatura: ");
    Serial.print(celsius);
    Serial.println(" c ");
    Serial.println("    ");
    
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
  

  delay(1000);
}
```

# Autor

Gilson Dias Ramos Junior

www.linkedin.com/in/gilson-dias-jn
