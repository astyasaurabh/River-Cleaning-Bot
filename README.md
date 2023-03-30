# River-Cleaning-Bot

#include <LiquidCrystal.h>


// second stage send data via wifi

#include <CytronWiFiShield.h>
#include <CytronWiFiClient.h>
#include <CytronWiFiServer.h>
#include <SoftwareSerial.h>
#include <TextFinder.h>
#include <ThingSpeak.h>  
unsigned long mychannelNumber = ".....";
const char *myWriteAPIKey = "......";

#include "Ultrasonic.h"
#include "Wire.h" //For 12C
//#include  "Liquidcrystal_l2C.h" //add library*
ESP32 client client;
int ph  = analogRead;//read ph value
int turbidity = analogRead(A2); //read turbidity value
const int usonic2_trig_pin=7;
const int usonic2_echo_pin=6;

const char *ssid = ".....";
const char *pass = ".....";

String readString = String(100); //string for fetching data from address
String ultrasonic_Value;
float sensor_reading;

int timer_count;

int turbudity_pin = analogRead(A2);
int ph_ph = analogRead);
int turbudity_val;
int ph_val;
float ph_val_f;

String myStatus ="";

LiquidCrystal _12c lcd(0x27,2,1,0,4,5,6,7); 

Ultrasonic ultrasonic(Usonic2_trig_pin,Usonic2_echo_pin);

void setup() {
 lcd.begin(20,4);
 lcd.setBacklightPin(3,POSITIVE); //BL, BL_POL
 lcd.setBacklightPin(HIGH);
 delay(1000);
 Configurewifi();
 ThingSpeak.begin(client)

   Serial.begin(115200);
   timer_count=0;
  }

  void loop()
  {sensor_reading = ultrasonic.Ranging(CM);
  delay(250);
   if (sensor_reading >0.0)}
   {
    turbudity_val=analogRead(turbidity_pin);
    delay(200);
    ph_val=analogRead(ph_pin);
    ph_val_f=float(614.333 -ph_val)/float(37.3333);
    delay(200);
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("Water Level(CM):");
    lcd.print(int(sensor_reading));
    //delay(50);
    //lcd.print("cm");
    lcd.setCursor (0,1);
    lcd.print("Turbudity :");
    lcd.print(turbudity_val);
    lcd,setCursor(0,2);
    lcd.print("ph value : ");
    lcd.print (ph_val_f);
    delay(500);

        Seial println(timer_count);
   }

   timer_count++;
   //Serial.println(button_state);
   if (timer_count>30 &&sensor_reading >0.0)//adc key
   {
    Serial.println("begin sending data");
    //Serial println(adc_key_in);
    configurewifi ();
    Serial.println("timer(3600):");
    Serial.println(clientTest(sensor_reading, turbudity_val, ph_val_f));
    timer_count=0;
   }

   //Serial.print("Button Status:");
   //Button status();

   void configurewifi()
   {
    //put your setup code here,to run once:
   
    Serial.begin(115200);
     if (!wifi.begin(2,3)) //wifi begin serial3
     {
      Serial.println(F("Error talking to shield"));
      lcd.clear();
      lcd.setCursor(0,0);
      lcd.print("Shield Error");
      delay(100);
      //While(1);
     }
     Serial.println(wifi.firmwareVersion());
     Serial.println(F("Mode:"));Serial.println(wifi.getMode());//Station mode,2-sof
     Serial.println(F("IP address:"));
     Serial.println(wifi.updateStatus();
     //lcd.clear();
     lcd.setCursor(0,3);
     lcd.print("wifi ok");
     delay(100);
     Serial.println(wifi.status()); //2-wifi connected withip ,3 
     ThingSpeak.begin(client);
      
    }
    String clientTest(int usonic_val, int turbidity_val, float ph_val)
    {
      ThingSpeak.setField(1, usonic_val);
      ThingSpeak.setField(2, turbidity_val);
      ThingSpeak.setField(3,ph_val);

      ThingSpeak.setStatus(myStatus);
      //write to thingspeak channel
      int x = ThingSpeak.writeFields(mychannelNumber, myWriteAPIKey);
      if(x ==200){
        Serial.println("Channel update successful.");
        lcd.setCursor(0,3);
        lcd.print("data sent");
        }
        else{
          Serial.println("problem updating channel. HTTP error code " + String(x));
          lcd.setCursor(0,3);
          lcd.println("connection issue");
        }
        delay(20000);
        client.stop();
        return mystatus;
    }
