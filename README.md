#include "DHT.h"

#define DHTPIN 2      // Пин, к которому подключен датчик
#define DHTTYPE DHT11 // Модель датчика: DHT11 или DHT22
#define RELAY_PIN 7   // Пин, к которому подключено реле
#define HUMIDITY_THRESHOLD 35  // Пороговое значение влажности

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);
  dht.begin();
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, LOW); // Реле выключено по умолчанию
}

void loop() {
  delay(2000);

  float humidity = dht.readHumidity();
  float temperature = dht.readTemperature();

  if (isnan(humidity) || isnan(temperature)) {
    Serial.println("Ошибка при чтении данных с датчика!");
    return;
  }

  Serial.print("Влажность: ");
  Serial.print(humidity);
  Serial.print(" %\t");
  Serial.print("Температура: ");
  Serial.print(temperature);
  Serial.println(" *C");

  // Проверка уровня влажности и управление реле
  if (humidity >= HUMIDITY_THRESHOLD) {
    digitalWrite(RELAY_PIN, HIGH); // Включение реле
    Serial.println("Реле включено");
  } else {
    digitalWrite(RELAY_PIN, LOW);  // Выключение реле
    Serial.println("Реле выключено");
  }
}
