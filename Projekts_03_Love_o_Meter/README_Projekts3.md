## Projekts 2 "Spaceship Interface"
```cpp
const int sensorPin = A0;
const float baselineTemp = 25.0;
void setup(){
  Serial.begin(9060);
  for(int pinNumber = 2; pinNumber<5; pinNumber++){
    pinMode(pinNumber,OUTPUT);
    digitalWrite(pinNumber, LOW);
  }
}
void loop(){
  int sensorVal = analogRead(sensorPin);
  Serial.print("Sensor value: ");
  Serial.print(sensorVal);
  float voltage = (sensorVal/1024.0) * 5.0;
  Serial.print(", Volts: ");
  Serial.print(voltage);
  Serial.print(", degrees C: ");
  float temperature = (voltage - .5) * 100;
  Serial.println(temperature);
  if(temperature < baselineTemp){
    digitalWrite(2, LOW);
    digitalWrite(3, LOW);
    digitalWrite(4, LOW);
  }
  else if(temperature >= baselineTemp+2 && temperature < baselineTemp+4){
    digitalWrite(2, HIGH);
    digitalWrite(3, LOW);
    digitalWrite(4, LOW);
  }
  else if(temperature >= baselineTemp+4 && temperature < baselineTemp+6){
    digitalWrite(2, HIGH);
    digitalWrite(3, HIGH);
    digitalWrite(4, LOW);
  }
  else if(temperature >= baselineTemp+6){
    digitalWrite(2, HIGH);
    digitalWrite(3, HIGH);
    digitalWrite(4, HIGH);
  }
  delay(1);
}
```

### Kosmoskuģa interfeis

1. Kosmoskuģa interfeis.
    - Darba izmantojas
        - 3 LED lampiņa sarkana krāsa
        - 3 Resistors uz 220om
        - TMP temperaturas sensors

Izvediojam shēmu kā gramata un pieslēdzam pie datora ar USB vadu, atvēram redaktoru, pāarakstam programas kodu saglabajam uz arduino

-----

- Rezultatā jabūt
    - termometras kas atkarīgi no temperaturas iededzinā 1 vai 2 vai 3 lampiņas, no sākuma deg tikai viena lampiņa, saspiežot temperaturas ar pirkstiem sensoru iedēgās pārejas lampiņas signalizējot kā temperatura pacēlas.

izmerā apkartējas vides temperaturu.
![3projekta 1](projekts_3_0_1.jpg)

izmēra pirkstu temperaturu.
![3projekta 2](projekts_3_0_2.jpg)
