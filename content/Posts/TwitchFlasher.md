---
{"publish":true,"created":"2025-08-24T19:15:39.582+01:00","modified":"2025-08-25T20:54:50.072+01:00","cssclasses":""}
---


```arduino
#include <Arduino.h>

#define LED 16 // pin 0

void setup() {
pinMode(LED, OUTPUT);    // LED pin as output.
}
void loop() {
digitalWrite(LED, HIGH);// turn the LED off.(Note that LOW is the voltage level but actually 
                        //the LED is on; this is because it is acive low on the ESP8266.
delay(50);            // wait for 1 second.
digitalWrite(LED, LOW); // turn the LED on.
delay(950); // wait for 1 second.
}
```

```arduino
#include <Arduino.h>

#include <ESP8266WiFi.h>
#include <PubSubClient.h>

#define LED 16 

const char* ssid = "TP-Link_F09";
const char* password = "14742781";
const char* mqtt_server = "192.168.0.106";
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
  Serial.print("Message arrived [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  // Switch on the LED if an 1 was received as first character
  if ((char)payload[0] == '1') {
    digitalWrite(LED, HIGH);// turn the LED off.(Note that LOW is the voltage level but actually  
    delay(50);            // wait for 1 second.
    digitalWrite(LED, LOW); // turn the LED on.
  } 
  else if ((char)payload[0] == '2') {
  digitalWrite(LED, HIGH);// turn the LED off.(Note that LOW is the voltage level but actually  
    delay(10);            // wait for 1 second.
    digitalWrite(LED, LOW); // turn the LED on.
}
  else if ((char)payload[0] == '3') {
    digitalWrite(LED, HIGH);// turn the LED off.(Note that LOW is the voltage level but actually  
    delay(500);            // wait for 1 second.
    digitalWrite(LED, LOW); // turn the LED on.
}
else if ((char)payload[0] == '4') {
   digitalWrite(LED, HIGH);// turn the LED off.(Note that LOW is the voltage level but actually  
    delay(50);            // wait for 1 second.
    digitalWrite(LED, LOW); // turn the LED on.
    delay(100);  
     digitalWrite(LED, HIGH);// turn the LED off.(Note that LOW is the voltage level but actually  
    delay(50);            // wait for 1 second.
    digitalWrite(LED, LOW); // turn the LED on.
    delay(100);  
     digitalWrite(LED, HIGH);// turn the LED off.(Note that LOW is the voltage level but actually  
    delay(50);            // wait for 1 second.
    digitalWrite(LED, LOW); // turn the LED on.
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
}
```
![[attachments/Pasted image 20250821215651.png]]![[attachments/Pasted image 20250821215654.png]]![[attachments/Pasted image 20250821215658.png]]![[attachments/Pasted image 20250821215701.png]][InfoLive: Découverte de Node Red et création d'un Bot pour Twitch](https://www.youtube.com/watch?v=ZNMEX1pudYo&ab_channel=YorzianVoD)

[JUST THE TIP - Home Assistant Node-Red: Streamline Switch and Light Flows](https://www.youtube.com/watch?v=C6MdDJ3UprI&ab_channel=3ATIVEVFXStudio)