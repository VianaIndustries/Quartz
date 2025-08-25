---
{"publish":true,"created":"2025-08-24T19:15:39.560+01:00","modified":"2025-08-25T20:55:08.174+01:00","cssclasses":""}
---

- Code
    
    ```
    ; PlatformIO Project Configuration File
    ;
    ;   Build options: build flags, source filter
    ;   Upload options: custom upload port, speed and extra flags
    ;   Library options: dependencies, extra library storages
    ;   Advanced options: extra scripting
    ;
    ; Please visit documentation for the other options and examples
    ; https://docs.platformio.org/page/projectconf.html
    
    [env:esp07s]
    platform = espressif8266
    board = esp07s
    framework = arduino
    lib_deps = 
    	arduino-libraries/NTPClient@^3.2.1
    	paulstoffregen/Time@^1.6.1
    monitor_speed = 115200
    ```
    
    ```cpp
    #include <NTPClient.h>
    #include <ESP8266WiFi.h>
    #include <WiFiUdp.h>
    #include <TimeLib.h>
    
    int latchPin = 5;  // Latch pin of 74HC595 
    int clockPin = 4; // Clock pin of 74HC595 
    int dataPin = 2;  // Data pin of 74HC595 
    
    byte  leds = B00000000; ; 
    byte seg = B11111111;// Variable to hold the pattern of which LEDs are currently turned on or off
    byte dis[] ={0x3,0x9F,0x25,0xD,0x99,0x49,0x41,0x1F,0x1,0x19,0x11,0xC1,0x63,0xC1,0x61,0x71};
    byte pos[] = {8,4,2,1,128,64,32,16};
    bool flag = 1;
    
    const char* ssid = "TP-Link_F09A";
    const char* password = "14742781";
    
    char Time[] = "TIME:00:00:00";
    char Date[] = "DATE:00/00/2000";
    byte last_second, second_, minute_, hour_, day_, month_;
    int year_;
    
    WiFiUDP ntpUDP;
    NTPClient timeClient(ntpUDP);
    
    const long interval = 1000; 
    unsigned long previousMillis = 0;
    
    unsigned long unix_epoch;
    
    unsigned long epochBirthDay = 876009600;
    
    void updateShiftRegister()
    {
      digitalWrite(latchPin, LOW);
      shiftOut(dataPin, clockPin, LSBFIRST, leds);
      shiftOut(dataPin, clockPin, LSBFIRST, seg);
      digitalWrite(latchPin, HIGH);
    }
    
    void updateTime(){
      unsigned long currentMillis = millis();
    
      if (currentMillis - previousMillis >= interval) {
        previousMillis = currentMillis;
      timeClient.update();
      }
    
      unix_epoch = timeClient.getEpochTime();
    }
    
    void displayTime(){
     second_ = second(unix_epoch);
      
        minute_ = minute(unix_epoch);
        hour_   = hour(unix_epoch);
      
    
        leds = dis[hour_   / 10 ];
        seg=   pos[0];
        updateShiftRegister();
        
        leds = dis[(hour_   % 10)+ 1 ];
        seg=   pos[1];
        updateShiftRegister();
        ;
        leds = dis[minute_ / 10 ];
        seg=   pos[3];
        updateShiftRegister();
        
        leds = dis[minute_ % 10 ];
        seg=   pos[4];
        updateShiftRegister();
        
        leds = dis[second_ / 10];
        seg=   pos[6];
        updateShiftRegister();
        
        leds = dis[second_ % 10 ];
        seg=   pos[7];
        updateShiftRegister();
    
    }
    
    void age(){
        unsigned long dif = unix_epoch - epochBirthDay;
        float epochAge = dif / 31556926.0000;
        
      
      leds = dis[(int)(epochAge   / 10)];
      seg=   pos[0];
      updateShiftRegister();
      
      leds = dis[(int)fmod(epochAge  , 10)] & B11111110;
      seg=   pos[1];
      updateShiftRegister();
      
      int dec = epochAge * 1000000;
      Serial.println(dec);
    
      leds = dis[(dec -((dec/1000000)*1000000))/100000] ;
      seg=   pos[2];
      updateShiftRegister();
    
      leds = dis[(dec -((dec/100000)*100000))/10000] ;
      seg=   pos[3];
      updateShiftRegister();
    
      leds = dis[(dec -((dec/10000)*10000))/1000] ;
      seg=   pos[4];
      updateShiftRegister();
    
      leds = dis[(dec -((dec/1000)*1000))/100] ;
      seg=   pos[5];
      updateShiftRegister();
    
      leds = dis[(dec -((dec/100)*100))/10] ;
      seg=   pos[6];
      updateShiftRegister();
    
      leds = dis[(dec -((dec/10)*10))] ;
      seg=   pos[7];
      updateShiftRegister();
     
     
    }
    
    void setup(){
      pinMode(latchPin, OUTPUT);
      pinMode(dataPin, OUTPUT);  
      pinMode(clockPin, OUTPUT);
      Serial.begin(115200);
    
      WiFi.begin(ssid, password);
    
      while ( WiFi.status() != WL_CONNECTED ) {
        delay ( 500 );
        
      }
    
      timeClient.begin();
      
    }
    
    void loop() {
      updateTime();
      age();
    }
    ```
    
- 💡 This code changes the second to last segment turning it off , this changes the bit to 1 that is off ```python byte dis[] ={0x3,0x9F,0x25,0xD,0x99,0x49,0x41,0x1F,0x1,0x19,0x11,0xC1,0x63,0xC1,0x61,0x71}; leds = dis[(int)fmod(epochAge , 10)] & B11111101 | B00000010; ```
    

1 year = 31556926 seconds

0.1=36.52 days

0.01=87.66 hours

0.001=8.76 hours

0,0001=52.58 minutes

0.00001=5.26 minutes

0.000001=31.56 seconds
![[attachments/Pasted image 20250821215603.jpg]]![[attachments/Pasted image 20250821215611.jpg]]![[attachments/Pasted image 20250821215623.jpg]]