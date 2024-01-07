#include <ESP8266WiFi.h>
#include <DHT.h>

#define DHTPIN 2
#define DHTTYPE DHT22

DHT dht(DHTPIN, DHTTYPE);

const char *ssid = "your-ssid";
const char *password = "your-password";
const char *thingSpeakApiKey = "your-thingspeak-api-key";
const char *thingSpeakAddress = "api.thingspeak.com";

#define DHTPIN 2
#define DHTTYPE DHT22

DHT dht(DHTPIN, DHTTYPE);

const int FAN_PIN = D1;
const int WATER_PUMP_PIN = D2;
const int LED_PIN = D3;
const int SOIL_MOISTURE_PIN = A0;  // Assuming soil moisture sensor is connected to analog pin A0

const float setpointTemperature = 25.0;   // Setpoint temperature in degrees Celsius
const float setpointHumidity = 60.0;      // Setpoint humidity in percentage
const int setpointSoilMoisture = 500;     // Setpoint soil moisture value (adjust based on sensor characteristics)

// PID parameters for each environmental parameter
const float KpTemperature = 5.0;
const float KiTemperature = 0.2;
const float KdTemperature = 1.0;

const float KpHumidity = 2.0;
const float KiHumidity = 0.1;
const float KdHumidity = 0.5;

const float KpSoilMoisture = 1.0;
const float KiSoilMoisture = 0.05;
const float KdSoilMoisture = 0.2;

float previousErrorTemperature = 0;
float integralTemperature = 0;

float previousErrorHumidity = 0;
float integralHumidity = 0;

float previousErrorSoilMoisture = 0;
float integralSoilMoisture = 0;

void sendDataToThingSpeak(float temperature, float humidity, int soilMoisture) {
  WiFiClient client;

  String data = "api_key=" + String(thingSpeakApiKey) +
                "&field1=" + String(temperature) +
                "&field2=" + String(humidity) +
                "&field3=" + String(soilMoisture);

  if (client.connect(thingSpeakAddress, 80)) {
    client.println("POST /update HTTP/1.1");
    client.println("Host: " + String(thingSpeakAddress));
    client.println("Connection: close");
    client.println("Content-Type: application/x-www-form-urlencoded");
    client.print("Content-Length: ");
    client.println(data.length());
    client.println();
    client.println(data);
    client.stop();
  }
}


void setup() {
  // Initialize sensors, actuators, and serial communication
  pinMode(FAN_PIN, OUTPUT);
  pinMode(WATER_PUMP_PIN, OUTPUT);
  pinMode(LED_PIN, OUTPUT);
  
  dht.begin();
  Serial.begin(115200);
}

void loop() {
  // Read sensor data
  float temperature = dht.readTemperature();
  float humidity = dht.readHumidity();
  int soilMoisture = analogRead(SOIL_MOISTURE_PIN);

  // Temperature PID control
  float errorTemperature = setpointTemperature - temperature;
  integralTemperature += errorTemperature;
  float derivativeTemperature = errorTemperature - previousErrorTemperature;
  float outputTemperature = KpTemperature * errorTemperature + KiTemperature * integralTemperature + KdTemperature * derivativeTemperature;

  // Humidity PID control
  float errorHumidity = setpointHumidity - humidity;
  integralHumidity += errorHumidity;
  float derivativeHumidity = errorHumidity - previousErrorHumidity;
  float outputHumidity = KpHumidity * errorHumidity + KiHumidity * integralHumidity + KdHumidity * derivativeHumidity;

  // Soil Moisture PID control
  float errorSoilMoisture = setpointSoilMoisture - soilMoisture;
  integralSoilMoisture += errorSoilMoisture;
  float derivativeSoilMoisture = errorSoilMoisture - previousErrorSoilMoisture;
  float outputSoilMoisture = KpSoilMoisture * errorSoilMoisture + KiSoilMoisture * integralSoilMoisture + KdSoilMoisture * derivativeSoilMoisture;

  // Implement control logic
  analogWrite(FAN_PIN, outputTemperature > 0 ? outputTemperature : 0);
  digitalWrite(WATER_PUMP_PIN, outputHumidity > 0);
  analogWrite(LED_PIN, outputSoilMoisture > 0 ? outputSoilMoisture : 0);

  // Display sensor readings and PID outputs
  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.print(" °C, Humidity: ");
  Serial.print(humidity);
  Serial.print(" %, Soil Moisture: ");
  Serial.print(soilMoisture);
  Serial.print(", PID Outputs: ");
  Serial.print(outputTemperature);
  Serial.print(", ");
  Serial.print(outputHumidity);
  Serial.print(", ");
  Serial.println(outputSoilMoisture);

  // Update previous errors
  previousErrorTemperature = errorTemperature;
  previousErrorHumidity = errorHumidity;
  previousErrorSoilMoisture = errorSoilMoisture;
  sendDataToThingSpeak(temperature, humidity, soilMoisture);

  delay(5000); // Delay for stability
}
