#include <WiFi.h>
#include <HTTPClient.h>
#include "DHTesp.h"
#include <ESP32Servo.h>

const int DHT_PIN = 15; //DHT is the temperature and humidity sensor

DHTesp dhtSensor; //delcarating the dhtsensor

const int servoPin = 18; //servo pin

Servo servo; //declarating the servo

//specifying the wifi credentials
const char* ssid = "Wokwi-GUEST"; 
const char* password = "";

HTTPClient http; //declarating the http
void setup() {
  Serial.begin(115200);
  
  //connecting to network
  WiFi.begin(ssid, password);

  Serial.print("Connecting to WiFi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.print("OK! IP=");
  Serial.println(WiFi.localIP());

  
  //sensors setup
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  servo.attach(servoPin, 250, 1200);
  servo.write(0);
  

}
int pos = 0;

  
void loop() {
  //sensors reading
  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 2) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");
  delay(1000);
  
  //conditioning for servo by the sensor value
  if(data.humidity < 15){
    for (pos = 0; pos <= 180; pos += 1) {
    servo.write(pos);
    delay(15);
    }

    delay(6000);

    for (pos = 180; pos >= 0; pos -= 1) {
      servo.write(pos);
      delay(15);
    }
  }


//begin to send data to server
http.begin("http://dcanteen.com/esp?temp="+String(data.temperature)+"&&humid="+String(data.humidity));
    int httpResponseCode = http.GET();
    if (httpResponseCode > 0) {
      Serial.print("HTTP ");
      Serial.println(httpResponseCode);
    }
    else {
      Serial.print("Error code: ");
      Serial.println(httpResponseCode);
      Serial.println(":-(");
    }
  delay(6000);
}
