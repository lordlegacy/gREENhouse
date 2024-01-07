# gREENhouse
Smart Greenhouse Monitoring System

# Overview
This project implements a smart greenhouse monitoring system using NodeMCU (ESP8266) to measure and control environmental parameters such as temperature, humidity, and soil moisture. The collected data is sent to ThingSpeak, an IoT platform, for storage and visualization.

# Features
Real-time monitoring of temperature, humidity, and soil moisture.
PID control for maintaining setpoint values of temperature, humidity, and soil moisture.
Integration with ThingSpeak for cloud-based data storage and visualization.

# Components
NodeMCU (ESP8266)
DHT22 sensor for temperature and humidity
Soil moisture sensor
ThingSpeak account for cloud data storage

# Getting Started
Set up your ThingSpeak account and create a new channel with fields for temperature, humidity, and soil moisture.
Update the NodeMCU code with your Wi-Fi credentials, ThingSpeak API key, and channel details.
Flash the updated code to your NodeMCU.
Power on the NodeMCU and monitor the environmental parameters on your ThingSpeak channel.

# Configuration
Wi-Fi Credentials: Set your Wi-Fi SSID and password in the code.
ThingSpeak API Key: Obtain your ThingSpeak API key from your ThingSpeak account.
ThingSpeak Channel Details: Update the channel ID and field numbers in the code.

# Dependencies
DHT Sensor Library for interfacing with the DHT22 sensor.

# Acknowledgments
Inspired by the need for efficient greenhouse monitoring and control.
