# Global Solution
# Sensor de Oxigênio Dissolvido na Água
Este projeto utiliza um microcontrolador Arduino para monitorar os níveis de oxigênio dissolvido (OD) em uma solução aquosa. Em vez de um sensor de OD real, um potenciômetro é utilizado para simular os diferentes níveis de oxigênio. Os dados lidos são exibidos em um display LCD e também são enviados via comunicação serial para um computador.

## Funcionalidades:
O sistema monitora a seguinte grandeza:
Oxigênio dissolvido na água

## Objetivos do Projeto
- Simular a leitura de um sensor de oxigênio dissolvido usando um potenciômetro.
- Exibir os dados lidos em um display LCD 16x2.
- Enviar os dados lidos via comunicação serial para um computador.

## Componentes necessários
| Componente    | Quantidade    |
| ------------- | ------------- |
| Arduino Uno R3  | 1 |
| Potenciômetro | 2  |
| LCD | 1 |

## Conexão dos Componentes
Conecte os componentes conforme o esquema de conexão fornecido no código-fonte.

## Utilização
Após a configuração e conexão dos componentes, abra o arquivo do código-fonte em um ambiente de desenvolvimento integrado para Arduino (como Arduino Uno), inicie o monitoramento executando o código no Arduino.

## LCD 
O LCD mostrará os valores captados pelo sensor, que também estarão na memória do aplicativo.
![image](https://github.com/Nicolejelinski/Global-Solution---Edge/assets/143125546/16ec2e0d-911e-4a29-9647-9b01903101b6)


## Notas Adicionais
- Conversão de Voltagem: A leitura do sensor analógico é convertida para milivolts utilizando a fórmula (LeituraSensorOD / 1023.0) * 5000. Esta fórmula assume uma referência de 5V para o ADC (Conversor Analógico para Digital) do Arduino.
- Equação do Sensor: A conversão de voltagem para mg/L de OD foi simplificada como OD = VoltagemSensorOD / 800. Ajuste esta equação conforme necessário para um sensor de OD real.


## Melhorias Futuras
- Implementar um Sensor de OD Real: Substituir o potenciômetro por um sensor de OD real.
- Calibração: Adicionar um procedimento de calibração para ajustar as leituras do sensor.
- Alerta Visual/Sonoro: Implementar LEDs ou um buzzer para alertar quando os níveis de OD estiverem fora dos limites desejados.

## Autores
Projeto desenvolvido para a matéria de Edge Computing do Professor Fabio Cabrini, por: 
Nicolle Pellegrino Jelinski;


# Code

```c++
// Incluindo biblioteca que possibilita a utilização do LCD
#include <LiquidCrystal.h>
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// Definindo parâmetros para motor e sensor de OD
const int PinSensorOD = A5;

// Iniciando variáveis para leitura do OD
int LeituraSensorOD;
float OD, VoltagemSensorOD = 0;

void setup()
{
  pinMode(9, OUTPUT);
  pinMode(8, OUTPUT);
  Serial.begin(9600); // Iniciei a comunicação serial
  delay(100);
  pinMode(PinSensorOD, INPUT);
  lcd.begin(16, 2);
  lcd.print("O2 Dissolvido = "); // print OD
}

void loop()
{
  // Variável OD
  LeituraSensorOD = analogRead(PinSensorOD); // Lê o sensor de OD
  VoltagemSensorOD = (LeituraSensorOD / 1023.0) * 5000;  // Converte para milivolts corretamente
  OD = VoltagemSensorOD / 800;  // Equação do sensor de OD

  // LCD (print)
  lcd.setCursor(0, 1);
  lcd.print(OD);
  lcd.print("mg/L   ");
  delay(1000); // delay 1 segundo

  // Comunicação serial
  if (OD <= 5.0) {
    Serial.println("O nível oxigenio está muito baixo.");
  } else {
    Serial.println("O nível oxigenio está dentro do padrão.");
  }
  Serial.print("O oxigenio atual esta em: ");
  Serial.print(OD);
  Serial.println("mg/L");
}
```
