## BLYNK CODE:
```cpp
#define BLYNK_TEMPLATE_ID "TMPL3bgdI4-00"
#define BLYNK_TEMPLATE_NAME "weather station"
#define BLYNK_AUTH_TOKEN "KFar-VbBPVpbqF8r-ZmZuqlWNLdYgec7"

#define BLYNK_PRINT Serial

#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <DHT.h>

char ssid[] = "@@@@@@@5714018716";
char pass[] = "*****389";

#define DHTPIN D4
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

BlynkTimer timer;

// Function to send sensor data
void sendSensor()
{
  float humidity = dht.readHumidity();
  float temperature = dht.readTemperature();

  if (isnan(humidity) || isnan(temperature))
  {
    Serial.println("Failed to read from DHT11!");
    return;
  }

  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.print(" °C\t");

  Serial.print("Humidity: ");
  Serial.print(humidity);
  Serial.println(" %");

  Blynk.virtualWrite(V1, temperature);
  Blynk.virtualWrite(V2, humidity);
}

void setup()
{
  Serial.begin(115200);

  dht.begin();

  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);

  // Read sensor every 2 seconds
  timer.setInterval(2000L, sendSensor);
}

void loop()
{
  Blynk.run();
  timer.run();
}
```


## ThingSpeak code:
```cpp
#include<ESP8266WiFi.h>
#include<DHT.h>
#include<ThingSpeak.h>

const char *ssid="********5714018716";
const char *pass="$$$$5389";

#define dht_type DHT11
#define dht_pin D4
long channelnum=3420641;
const char writeapikey[] ="UPZAZG5YV7AHWZK7";
WiFiClient client;
DHT dht(dht_pin,dht_type);
void setup()
{
Serial.begin(115200);
WiFi.begin(ssid,pass);
while(WiFi.status()!=WL_CONNECTED)
{
  Serial.print("...");
  delay(200);
}
Serial.println("node mcu is connected");
Serial.println(WiFi.localIP());
dht.begin();
ThingSpeak.begin(client);
}



void loop()
{
 float t=dht.readTemperature();
 float h=dht.readHumidity();

 Serial.print("Temperature:");
 Serial.print(t);
 Serial.println("*C");

 Serial.print("Humidity");
 Serial.print(h);
 Serial.println("%");

ThingSpeak.writeField(channelnum,1,t,writeapikey);
ThingSpeak.writeField(channelnum,2,h,writeapikey);
delay(200);
 
}


```
