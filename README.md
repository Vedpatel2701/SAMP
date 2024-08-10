#include <WiFi.h>
#include <HTTPClient.h>

const char* ssid = "your_ssid";
const char* password = "your_password";

// Sensor pins
const int soilMoisturePin = A0;
const int pHPin = A1;
const int tempHumidityPin = D2;

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Serial.println("Connected to WiFi");
}

void loop() {
  int soilMoisture = analogRead(soilMoisturePin);
  float pH = analogRead(pHPin) * 3.3 / 4096;
  float temperature = dht.readTemperature();
  float humidity = dht.readHumidity();

  // Send data to cloud/server
  HTTPClient http;
  http.begin("http://your_server_ip/soil_data");
  http.addHeader("Content-Type", "application/json");
  String jsonData = "{\"soil_moisture\": " + String(soilMoisture) + ", \"pH\": " + String(pH) + ", \"temperature\": " + String(temperature) + ", \"humidity\": " + String(humidity) + "}";
  http.POST(jsonData);
  http.end();

  delay(60000); // Send data every 1 minute
}
















//FOR DATA ANALYTICS
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import train_test_split

# Load data from database
data = pd.read_sql_query("SELECT * FROM soil_data", db_connection)

# Calculate soil health index
def calculate_soil_health(data):
  # Your calculation logic here
  return soil_health_index

# Detect anomalies
def detect_anomalies(data):
  # Your anomaly detection logic here
  return anomalies

# Predict fertilizer/irrigation needs
def predict_needs(data):
  # Your prediction logic here
  return predictions

# Send alerts/notifications
def send_alerts(anomalies, predictions):
  # Your alert/notification logic here
  pass

# Display data on dashboard/web interface
def display_data(data):
  # Your dashboard/web interface logic here
  pass

# Call functions
soil_health_index = calculate_soil_health(data)
anomalies = detect_anomalies(data)
predictions = predict_needs(data)
send_alerts(anomalies, predictions)
display_data(data)












// WEB APP CODE
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import { Line } from 'react-chartjs-2';
import { Chart as ChartJS, CategoryScale, LinearScale, PointElement, LineElement, Title, Tooltip, Legend } from 'chart.js';

ChartJS.register(CategoryScale, LinearScale, PointElement, LineElement, Title, Tooltip, Legend);

const App = () => {
    const [data, setData] = useState([]);
    
    useEffect(() => {
        axios.get('http://localhost:5000/api/data')
            .then(response => setData(response.data))
            .catch(error => console.error('Error fetching data:', error));
    }, []);

    const chartData = {
        labels: data.map(d => d.timestamp),
        datasets: [
            {
                label: 'Moisture',
                data: data.map(d => d.moisture),
                borderColor: 'rgba(75,192,192,1)',
                backgroundColor: 'rgba(75,192,192,0.2)',
                fill: true,
            },
            {
                label: 'pH',
                data: data.map(d => d.pH),
                borderColor: 'rgba(255,99,132,1)',
                backgroundColor: 'rgba(255,99,132,0.2)',
                fill: true,
            },
            {
                label: 'Temperature',
                data: data.map(d => d.temperature),
                borderColor: 'rgba(153,102,255,1)',
                backgroundColor: 'rgba(153,102,255,0.2)',
                fill: true,
            }
        ]
    };

    return (
        <div className="App">
            <h1>Soil Health Monitoring Dashboard</h1>
            <Line data={chartData} />
        </div>
    );
};

export default App;
