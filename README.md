# IoT-based-rockfall-detection-system-using-vibration-sensors.
This project focuses on building such a system, combining vibration-sensing technology, embedded systems, and cloud or mobile communication, to create a practical, low-cost early warning solution.
/*
  Project: Rockfall Detection System
  Microcontroller: ESP32
  Sensor: SW-420 Vibration Sensor
  Outputs: Buzzer and LED
*/

#define VIBRATION_SENSOR 27
#define BUZZER_PIN 25
#define LED_PIN 26

#define SENSOR_ACTIVE_STATE LOW

const unsigned long ALARM_HOLD_TIME = 2000;

unsigned long lastVibrationTime = 0;
bool alarmActive = false;

void setup() {
  Serial.begin(115200);

  pinMode(VIBRATION_SENSOR, INPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  pinMode(LED_PIN, OUTPUT);

  digitalWrite(BUZZER_PIN, LOW);
  digitalWrite(LED_PIN, LOW);

  Serial.println("================================");
  Serial.println(" ROCKFALL DETECTION SYSTEM");
  Serial.println(" ESP32 + SW-420 SENSOR");
  Serial.println("================================");
  Serial.println("System initialized successfully");
}

void loop() {

  int sensorValue = digitalRead(VIBRATION_SENSOR);

  // Detect vibration
  if (sensorValue == SENSOR_ACTIVE_STATE) {

    lastVibrationTime = millis();

    if (!alarmActive) {
      Serial.println("WARNING: VIBRATION DETECTED!");
      Serial.println("Possible rockfall activity!");
    }

    alarmActive = true;
  }

  // Stop alarm after vibration ends
  if (alarmActive &&
      millis() - lastVibrationTime >= ALARM_HOLD_TIME) {

    alarmActive = false;

    Serial.println("Vibration stopped.");
    Serial.println("System back to monitoring mode.");
  }

  // Control buzzer and LED
  if (alarmActive) {
    digitalWrite(BUZZER_PIN, HIGH);
    digitalWrite(LED_PIN, HIGH);
  }
  else {
    digitalWrite(BUZZER_PIN, LOW);
    digitalWrite(LED_PIN, LOW);
  }

  delay(10);
}
