# Cod
#include <common.h>
#include <Firebase.h>
#include <FirebaseESP32.h>
#include <FirebaseFS.h>
#include <Utils.h>


#include <WiFi.h>
#include <WiFiClient.h>
#include <FirebaseESP32.h>




#include <ArduinoJson.h>


#define DEBUG_SW 1 // Make it 1 to see all debug messages in Serial Monitor

                                                                                            //
// Firebase Credentials
#define FIREBASE_HOST "tttggg-bb2d7-default-rtdb.firebaseio.com"
#define FIREBASE_AUTH "Z2H1TIqxVBNzzvw2ncKdr7RxTROWM2E7DyMFTIdZ"

// WiFi Credentials
#define WIFI_SSID "111ttt333"         //     
#define WIFI_PASSWORD "tttt5555"

// Function Declaration
void  Sent_data_to_firebase();
//ultra1
#define trig1 2       //trig out
#define echo1 4        //echo in
//ultra2                
#define trig2 18       //trig out                                                                      // 
#define echo2 19        //echo in
//ultra3
#define trig3 23      //trig out
#define echo3 22       //echo in


//Define FirebaseESP32 data object
FirebaseData firebaseData;
FirebaseJson json;


void setup()
{
  pinMode(trig1,OUTPUT);//Trigger
  pinMode(echo1,INPUT);//Echo

  pinMode(trig2,OUTPUT);//Trigger
  pinMode(echo2,INPUT);//Echo

  pinMode(trig3,OUTPUT);//Trigger
  pinMode(echo3,INPUT);//Echo

  Serial.begin(115200);
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);

  Firebase.begin(FIREBASE_HOST, FIREBASE_AUTH);
  Firebase.reconnectWiFi(true);

}


void loop()
{

  if (WiFi.status() != WL_CONNECTED)
  {
//      Serial.println("Not Connected");
  }
  else
  {
    Sent_data_to_firebase();
  }
}
void Sent_data_to_firebase()
{
      String Ultra_Value1;
      String Ultra_Value2;
      String Ultra_Value3;
      
//---------------------------------------------------------------------------------------------------------------
     long T1,distance1;
     
     digitalWrite(trig1,LOW); 
     delayMicroseconds(2);  
     digitalWrite(trig1,HIGH);
     delayMicroseconds(10);  
     digitalWrite(trig1,LOW);
     
     T1=pulseIn(echo1,HIGH);                                                        //right 1
     distance1=(T1/2)/29.1;
     if (distance1 > 100 || distance1 < 2)
     {
      Serial.println("out of range right");

//send data to firebase---0
      String Ultra_Value1 = "0";
        json.set("/Right", Ultra_Value1);
        Firebase.set(firebaseData, "/ULTRA", json);
        Serial.println("Firebase RIGHT - 0");
     }
     else{
      Serial.print("ULTRA Right is..");
      Serial.print(distance1);
      Serial.println("cm");
      
//send data to firebase---1
      String Ultra_Value1 = "1";
       json.set("/Right", Ultra_Value1);
       Firebase.updateNode(firebaseData, "/ULTRA", json);
       Serial.println("Firebase RIGHT - 1"); 
      }
      delay(1500);
//-------------------------------------------------------------------------------------------------------------------
     long T2,distance2;
     
     digitalWrite(trig2,LOW); 
     delayMicroseconds(2);  
     digitalWrite(trig2,HIGH);
     delayMicroseconds(10);  
     digitalWrite(trig2,LOW);
     
     T2=pulseIn(echo2,HIGH);                                                        //left 2
     distance2=(T2/2)/29.1;
     if (distance2 > 100 || distance2 < 2){
     Serial.println("out of range left");

//send data to firebase---0
      String Ultra_Value2 = "0";
       json.set("/Left", Ultra_Value2);
       Firebase.set(firebaseData, "/ULTRA", json);
       Serial.println("Firebase left - 0");
     }
     else{
      Serial.print("ULTRA Left is..");
      Serial.print(distance2);
      Serial.println("cm");

//send data to firebase---1
      String Ultra_Value2 = "1";
       json.set("/Left", Ultra_Value2);
       Firebase.updateNode(firebaseData, "/ULTRA", json);
       Serial.println("Firebase left - 1");
      }
      delay(1000);
//-------------------------------------------------------------------------------------------------------------------
     long T3,distance3;
     
     digitalWrite(trig3,LOW); 
     delayMicroseconds(2);  
     digitalWrite(trig3,HIGH);
     delayMicroseconds(10);  
     digitalWrite(trig3,LOW);
     
     T3=pulseIn(echo3,HIGH);                                                        //forword 3
     distance3=(T3/2)/29.1;
     if (distance3 > 100 || distance3 < 2){
     Serial.println("out of range forword");

//send data to firebase---0
      String Ultra_Value3 = "0";
       json.set("/Forword", Ultra_Value3);
       Firebase.set(firebaseData, "/ULTRA", json);
       Serial.println("Firebase forword - 0");
     }
     else{
      Serial.print("ULTRA Forword is..");
      Serial.print(distance3);
      Serial.println("cm");

//send data to firebase---1
      String Ultra_Value3 = "1";
       json.set("/Forword", Ultra_Value3);
       Firebase.updateNode(firebaseData, "/ULTRA", json);
       Serial.println("Firebase forword - 1");
      }
      delay(500);
  }
