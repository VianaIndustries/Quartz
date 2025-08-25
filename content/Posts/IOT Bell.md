---
{"publish":true,"created":"2025-08-24T19:15:39.565+01:00","modified":"2025-08-25T20:55:05.191+01:00","cssclasses":""}
---

# Arduino Basic

```arduino
#include <Arduino.h>

void setup() {

pinMode(LED_BUILTIN, OUTPUT);

}

void loop() {

digitalWrite(LED_BUILTIN, HIGH);   // turn the LED on (HIGH is the voltage level)

delay(50);                       // wait for a second

digitalWrite(LED_BUILTIN, LOW);    // turn the LED off by making the voltage LOW

delay(500);                       // wait for a second

}
```

```arduino
; PlatformIO Project Configuration File
;
;   Build options: build flags, source filter
;   Upload options: custom upload port, speed and extra flags
;   Library options: dependencies, extra library storages
;   Advanced options: extra scripting
;
; Please visit documentation for the other options and examples
; https://docs.platformio.org/page/projectconf.html

[env:uno]
platform = atmelavr
board = uno
framework = arduino
```

# Circuit

![Untitled](app://1c3ca8907e5c8c34964f70eca0b9542fe6b5/C:/Users/Filipe/Desktop/HugoWebsite/Obsidian/Viana%20Industries/static/post3/Untitled.png?1696847690000)

# Material

IRFZ44N

12v mini solenoid

# Wemos D1 Uno

- test
    
    ```arduino
    ; PlatformIO Project Configuration File
    ;
    ;   Build options: build flags, source filter
    ;   Upload options: custom upload port, speed and extra flags
    ;   Library options: dependencies, extra library storages
    ;   Advanced options: extra scripting
    ;
    ; Please visit documentation for the other options and examples
    ; https://docs.platformio.org/page/projectconf.html
    
    [env:d1]
    platform = espressif8266
    board = d1
    framework = arduino
    ```
    
    ```arduino
    #include <Arduino.h>
    
    #include <ESP8266WiFi.h>
    #include <PubSubClient.h>
    
    #define LED 2  // ESP8266 CHIP LED PIN 9
    
    const char* ssid = "TP-Link_F09A";
    const char* password = "14742781";
    const char* mqtt_server = "192.168.1.2";
    String username = "mqtt";
    String passwordmqtt = "lkjhgfdsa1";
    String topic = "python/mqtt";
    WiFiClient espClient;
    PubSubClient client(espClient);
    unsigned long lastMsg = 0;
    #define MSG_BUFFER_SIZE	(50)
    char msg[MSG_BUFFER_SIZE];
    int value = 0;
    
    void setup_wifi() {
    
      delay(10);
      // We start by connecting to a WiFi network
      Serial.println();
      Serial.print("Connecting to ");
      Serial.println(ssid);
    
      WiFi.mode(WIFI_STA);
      WiFi.begin(ssid, password);
    
      while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
      }
    
      randomSeed(micros());
    
      Serial.println("");
      Serial.println("WiFi connected");
      Serial.println("IP address: ");
      Serial.println(WiFi.localIP());
    }
    
    void reconnect() {
      // Loop until we're reconnected
      while (!client.connected()) {
        Serial.print("Attempting MQTT connection...");
        // Create a random client ID
        String clientId = "ESP8266Client-";
        clientId += String(random(0xffff), HEX);
        // Attempt to connect
        if (client.connect(clientId.c_str(),"mqtt","lkjhgfdsa1")) {
          Serial.println("connected");
          // Once connected, publish an announcement...
          client.publish("python/mqtt", "Bell is online");
          // ... and resubscribe
          client.subscribe("python/mqtt");
        } else {
          Serial.print("failed, rc=");
          Serial.print(client.state());
          Serial.println(" try again in 5 seconds");
          // Wait 5 seconds before retrying
          delay(5000);
        }
      }
    }
    
    void callback(char* topic, byte* payload, unsigned int length) {
       char code[length];
      Serial.print("Message arrived [");
      Serial.print(topic);
      Serial.print("] ");
      for (int i = 0; i < length; i++) {
        Serial.print((char)payload[i]);
        code[i]=(char)payload[i];
      }
      Serial.println();
      Serial.println(code);
    
      // Switch on the LED if an 1 was received as first character
      if (strcmp(code, "11" )== 0) {
        digitalWrite(LED, LOW);// turn the LED off.(Note that LOW is the voltage level but actually  
        delay(50);            // wait for 1 second.
        digitalWrite(LED, HIGH); // turn the LED on.
      } 
      else if (strcmp(code, "12" )== 0) {
      digitalWrite(LED, LOW);// turn the LED off.(Note that LOW is the voltage level but actually  
        delay(25);            // wait for 1 second.
        digitalWrite(LED, HIGH); // turn the LED on.
    }
      else if (strcmp(code, "13" )== 0) {
        digitalWrite(LED, LOW);// turn the LED off.(Note that LOW is the voltage level but actually  
        delay(10);            // wait for 1 second.
        digitalWrite(LED, HIGH); // turn the LED on.
    }
    else if (strcmp(code, "14" )== 0) {
       digitalWrite(LED, LOW);// turn the LED off.(Note that LOW is the voltage level but actually  
        delay(50);            // wait for 1 second.
        digitalWrite(LED, HIGH); // turn the LED on.
        delay(100);  
         digitalWrite(LED, LOW);// turn the LED off.(Note that LOW is the voltage level but actually  
        delay(50);            // wait for 1 second.
        digitalWrite(LED, HIGH); // turn the LED on.
        delay(100);  
         digitalWrite(LED, LOW);// turn the LED off.(Note that LOW is the voltage level but actually  
        delay(50);            // wait for 1 second.
        digitalWrite(LED, HIGH); // turn the LED on.
    }
      else {
        digitalWrite(LED, LOW);  // Turn the LED off by making the voltage HIGH
      }
    }
    
    void setup() {
    pinMode(LED, OUTPUT);    // LED pin as output.
    Serial.begin(115200);
    setup_wifi();
    client.setServer(mqtt_server, 1883);
    client.setCallback(callback);
    
    }
    
    void loop() {
      if (!client.connected()) {
        reconnect();}
    
      client.loop();
    ```
    

## Final Code

**MQTT Code** : QQQTTTDDD , q is the quantity of cycles, t is the pulse time , d is the delay between pulses

```arduino
#include <Arduino.h>

#include <ESP8266WiFi.h>
#include <PubSubClient.h>

#define LED 2 

const char* ssid = "TP-Link_F09A";
const char* password = "14742781";
const char* mqtt_server = "192.168.1.2";
String username = "mqtt";
String passwordmqtt = "lkjhgfdsa1";
String topic = "python/mqtt";
WiFiClient espClient;
PubSubClient client(espClient);
unsigned long lastMsg = 0;
#define MSG_BUFFER_SIZE	(50)
char msg[MSG_BUFFER_SIZE];
int value = 0;

void setup_wifi() {

  delay(10);
  // We start by connecting to a WiFi network
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  randomSeed(micros());

  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}

void reconnect() {
  // Loop until we're reconnected
  while (!client.connected()) {
    Serial.print("Attempting MQTT connection...");
    // Create a random client ID
    String clientId = "ESP8266Client-";
    clientId += String(random(0xffff), HEX);
    // Attempt to connect
    if (client.connect(clientId.c_str(),"mqtt","lkjhgfdsa1")) {
      Serial.println("connected");
      // Once connected, publish an announcement...
      client.publish("python/mqtt", "Bell is online");
      // ... and resubscribe
      client.subscribe("python/mqtt");
    } else {
      Serial.print("failed, rc=");
      Serial.print(client.state());
      Serial.println(" try again in 5 seconds");
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}

void callback(char* topic, byte* payload, unsigned int length) {
   char code[length];
  Serial.print("Message arrived [");
  Serial.print(topic);
  Serial.print("] ");

  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
    code[i]=(char)payload[i];
  }
  Serial.println();
 

  int codeInt = atoi(code);
  int quant = codeInt/1000000;
  int time = (codeInt - quant * 1000000)/1000;
  int timeDelay = (codeInt - quant * 1000000) - time * 1000;
  Serial.println(quant);
  Serial.println(time);
  Serial.println(timeDelay);
  for(int x = 0; x < quant; x++){
    digitalWrite(LED, HIGH);// turn the LED off.(Note that LOW is the voltage level but actually  
    delay(time);            // wait for 1 second.
    digitalWrite(LED, LOW); // turn the LED on.
    Serial.println("blink");
    delay(time); 

  }
  Serial.println("DONE");

}

void setup() {
pinMode(LED, OUTPUT);    // LED pin as output.
Serial.begin(115200);
setup_wifi();
client.setServer(mqtt_server, 1883);
client.setCallback(callback);

}

void loop() {
  if (!client.connected()) {
    reconnect();}

  client.loop();
}
```

![[attachments/Pasted image 20250821214108.jpg]]