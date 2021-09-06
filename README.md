#include <String.h>
#include "DHTesp.h"
#define DHTPIN 1
DHTesp dht;
/* MQ-4 Methane Sensor Circuit with Arduino */
const int AOUTpin = 3; //the AOUT pin of the methane sensor goes into analog pin A0 of the arduino
const int ledPin = 2; //the anode of the LED connects to digital pin D13 of the arduino
int limit = 070;  //used to limit methane count
int value;
void setup()
{

  Serial.begin(115200);//braud rate
  pinMode(AOUTpin, INPUT);//sets the pin as an input to the arduino
  pinMode(ledPin, OUTPUT);//sets the pin as an output of the arduino
  dht.setup(DHTPIN, DHTesp::DHT11);
}
void loop()
{
  float h = dht.getHumidity();
  // Read temperature as Celsius (the default)
  float t = dht.getTemperature();
  Serial.println(dht.getStatusString());
  Serial.print("\t");
  Serial.print("Temperature: ");
  Serial.print( t);
  Serial.print("\t");
  Serial.print("Humidity: ");
  Serial.println(h);
  value = analogRead(AOUTpin); //reads the analaog value from the methane sensor's AOUT pin
  Serial.print("Methane value: ");
  Serial.print(value);//prints the methane value
  if (value > limit)
  {
    digitalWrite(ledPin, HIGH);//if limit has been reached, LED turns on as status indicator
    Serial.println(" Methane level is HIGH & FRUITS ARE GOING TO BE ROTTON SOON");
  }
  else
  {
    digitalWrite(ledPin, LOW);//if threshold not reached, LED remains off
    Serial.println(" Methane level is Normal & FRUITS ARE SAFE");
  }
  delay(10000);

}


