
# Projekts Nr. 11: Kristāla Lode (Crystal Ball)  

Šis projekts simulē "nākotnes paredzēšanas lodi", kas izmanto Arduino un akselerometru. Kad lietotājs sakrata ierīci, ekrānā tiek parādīts nejauši ģenerēts teksts — atbilde uz jautājumu.

---

## Materiāli
- **1 x Arduino Uno**  
- **1 x OLED ekrāns (I2C vai SPI)**  
- **1 x Akselerometrs (piem., ADXL345 vai MPU-6050)**  
- **Savienojošie vadi**  
- **USB kabelis Arduino pieslēgšanai datoram**  
- **Maizes dēlis (breadboard)**  

---

## Kods
Iekopējiet un augšupielādējiet zemāk redzamo koda fragmentu Arduino IDE:  

```cpp
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <Adafruit_MPU6050.h>
#include <Adafruit_Sensor.h>

// OLED konfigurācija
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire);

// MPU6050 akselerometra objekts
Adafruit_MPU6050 mpu;

// Atbildes masīvs
const char* atbildes[] = {
  "Jā",
  "Nē",
  "Varbūt",
  "Noteikti!",
  "Pajautā vēlāk",
  "Nav skaidrs"
};
int atbilzuSkaits = sizeof(atbildes) / sizeof(atbildes[0]);

void setup() {
  Serial.begin(9600);

  // Iniciējam OLED ekrānu
  if (!display.begin(SSD1306_I2C_ADDRESS, 0x3C)) {
    Serial.println("OLED ekrāns nav atrasts!");
    while (true);
  }
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);

  // Iniciējam MPU6050
  if (!mpu.begin()) {
    Serial.println("MPU6050 nav atrasts!");
    while (true);
  }
  display.display();
  delay(1000);
}

void loop() {
  // Nolasām akselerometra datus
  sensors_event_t a, g, temp;
  mpu.getEvent(&a, &g, &temp);

  // Pārbaudām, vai ierīce tika sakratīta
  if (abs(a.acceleration.x) > 2 || abs(a.acceleration.y) > 2 || abs(a.acceleration.z) > 2) {
    izveliesAtbildi();
    delay(1000); // Gaidām, lai nepieļautu vairākas atbildes uzreiz
  }
}

void izveliesAtbildi() {
  int randomIndex = random(0, atbilzuSkaits);
  display.clearDisplay();
  display.setCursor(0, 10);
  display.println("Atbilde:");
  display.setCursor(0, 30);
  display.println(atbildes[randomIndex]);
  display.display();
}
```

---

## Rezultāts  
Kad ierīce tiek sakratīta, ekrānā tiek parādīta nejauša atbilde no definētā saraksta. Akselerometrs reaģē uz kustību un ģenerē datu pārmaiņas, ko izmanto sakratīšanas noteikšanai.
![.]([Projekts_11_Crystal_Ball/Tinkercad%20simulacija.png](https://github.com/23DP2MPels/Arduino_DP2-2_MP/blob/master/Projekts_11_Crystal_Ball/Tinkercad%20simulacija.png))
