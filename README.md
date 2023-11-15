# Introduction-to-blynk-sent

Code:

```css
#define BLYNK_TEMPLATE_ID "TMPL6yCXpZXGh"
#define BLYNK_TEMPLATE_NAME "LED ESP32"
#define BLYNK_AUTH_TOKEN "oJoLVOu_PVkszDC8mcqpGr5izDWb9dQo"


#define BLYNK_PRINT Serial


#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>
#include "DHT.h"

char ssid[] = "OriO";
char pass[] = "Toeyyloli";

#define LED_PIN 2
#define DHTTYPE DHT11
#define DHTPIN 17

const int potPin = 34;
int potValue = 0;
const int luxPin = 39;
int luxValue = 0;
DHT dht(DHTPIN, DHTTYPE);
BlynkTimer timer;

BLYNK_WRITE(V0)
{
  int value = param.asInt();
  if (value == 1) {
    digitalWrite(LED_PIN, HIGH);
    Serial.print("value =");
    Serial.println(value);
  } else {
    digitalWrite(LED_PIN, LOW);
    Serial.print("value = ");
    Serial.println(value);
  }
}

void readPot()
{

  potValue = analogRead(potPin);
  Serial.println ("Pot Value = " + String(potValue / 40.95));
  Blynk.virtualWrite(V5, potValue);
  luxValue = analogRead(luxPin);
  Serial.println ("Lux Value = " + String(luxValue / 40.95));
  Blynk.virtualWrite(V6, luxValue);
  int h = dht.readHumidity();
  Serial.println ("Humidity Value = " + String(h));
  Blynk.virtualWrite(V7, h);
  float t = dht.readTemperature();
  Serial.println ("Temperature Value = " + String(t));
  Blynk.virtualWrite(V4, t);


}

BLYNK_CONNECTED()
{
  Blynk.setProperty(V3, "offImageUrl", "https://static-image.nyc3.cdn.digitaloceanspaces.com/general/fte/congratulations.png");
  Blynk.setProperty(V3, "onImageUrl",  "https://static-image.nyc3.cdn.digitaloceanspaces.com/general/fte/congratulations_pressed.png");
  Blynk.setProperty(V3, "url", "https://docs.blynk.io/en/getting-started/what-do-i-need-to-blynk/how-quickstart-device-was-made");
}

void myTimerEvent()
{
  Blynk.virtualWrite(V2, millis() / 1000);
}
void setup()
{
  dht.begin();
  Serial.begin(115200);
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
  timer.setInterval(1000L, myTimerEvent);
  timer.setInterval(2000L, readPot);
  Serial.println("Init Timer ");
  pinMode(LED_PIN, OUTPUT);
}

void loop()
{
  Blynk.run();
  timer.run();
  delay(10);
}
```
