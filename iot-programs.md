# Some IOT Programs

## Write a simple traffic light controller using an Arduino board with three LEDs connected to pins D5, D6, and D7, representing red, yellow, and green lights respectively.

```C
#define RED_PIN D5
#define YELLOW_PIN D6
#define GREEN_PIN D7

void setup() {
  // Initialize the LED pins as output
  pinMode(RED_PIN, OUTPUT);
  pinMode(YELLOW_PIN, OUTPUT);
  pinMode(GREEN_PIN, OUTPUT);

  // Start with the red light on
  digitalWrite(RED_PIN, HIGH);
  digitalWrite(YELLOW_PIN, LOW);
  digitalWrite(GREEN_PIN, LOW);
}

void loop() {
  // Red light on for 5 seconds
  digitalWrite(RED_PIN, HIGH);
  digitalWrite(YELLOW_PIN, LOW);
  digitalWrite(GREEN_PIN, LOW);
  delay(5000);

  // Yellow light on for 2 seconds
  digitalWrite(RED_PIN, LOW);
  digitalWrite(YELLOW_PIN, HIGH);
  digitalWrite(GREEN_PIN, LOW);
  delay(2000);

  // Green light on for 5 seconds
  digitalWrite(RED_PIN, LOW);
  digitalWrite(YELLOW_PIN, LOW);
  digitalWrite(GREEN_PIN, HIGH);
  delay(5000);
}
```

## Write a code for measuring distance using the HC-SR04 ultrasonic sensor.

```C
#define TRIG_PIN D1  // Trigger pin
#define ECHO_PIN D2  // Echo pin

void setup() {
  Serial.begin(115200);  // Initialize serial communication
  pinMode(TRIG_PIN, OUTPUT);  // Set the trig pin as an output
  pinMode(ECHO_PIN, INPUT);   // Set the echo pin as an input
}

void loop() {
  long duration, distance;

  // Clear the TRIG_PIN
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);

  // Send a 10us HIGH pulse to trigger the ultrasonic sensor
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  // Read the time it takes for the echo to return
  duration = pulseIn(ECHO_PIN, HIGH);

  // Calculate distance in centimeters
  distance = (duration * 0.0343) / 2;  // Speed of sound is ~343 m/s or 0.0343 cm/us

  // Print the distance to the Serial Monitor
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // Wait 500 milliseconds before the next reading
  delay(500);
}

```
### Explanation:
- ``TRIG_PIN``: Sends the ultrasonic pulse.
- ``ECHO_PIN``: Receives the reflected pulse.
- The code calculates the distance based on the duration the pulse takes to return and prints the result to the serial monitor.
- The delay(500) ensures the distance is measured every 500 milliseconds.

## Write a code for measuring humidity and temperature using the DHT11 or DHT22 sensor.

```C
#include "DHT.h"

#define DHTPIN D4  // Pin where the data pin of DHT sensor is connected
#define DHTTYPE DHT11  // Change to DHT22 if using the DHT22 sensor

DHT dht(DHTPIN, DHTTYPE);  // Initialize DHT sensor

void setup() {
  Serial.begin(115200);  // Start serial communication
  dht.begin();  // Initialize the DHT sensor
}

void loop() {
  // Wait a few seconds between measurements
  delay(2000);

  // Read humidity
  float humidity = dht.readHumidity();
  // Read temperature as Celsius
  float temperature = dht.readTemperature();

  // Check if any reads failed and exit early
  if (isnan(humidity) || isnan(temperature)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }

  // Print the humidity and temperature values to Serial Monitor
  Serial.print("Humidity: ");
  Serial.print(humidity);
  Serial.print(" %\t");
  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.println(" *C");
}
```

### Explanation:
- ``DHTPIN``: The data pin of the DHT sensor is connected to pin D4 on your board.
- ``DHTTYPE``: You can choose between DHT11 or DHT22 depending on your sensor.
- In the ``loop()`` function, the program reads the temperature and humidity every 2 seconds.
- ``isnan()`` checks whether the sensor reading failed, printing an error message if so.
- If the readings are successful, they are printed to the serial monitor.

## Write a code for controlling a buzzer using an Arduino board.

```C
#define BUZZER_PIN D1  // Pin where the buzzer is connected

void setup() {
  pinMode(BUZZER_PIN, OUTPUT);  // Set the buzzer pin as an output
}

void loop() {
  // Turn the buzzer on
  digitalWrite(BUZZER_PIN, HIGH);
  delay(1000);  // Wait for 1 second (1000 ms)

  // Turn the buzzer off
  digitalWrite(BUZZER_PIN, LOW);
  delay(1000);  // Wait for 1 second before the next loop
}
```
### Explanation:
- ``BUZZER_PIN``: This defines the pin connected to the buzzer, in this case, pin D1.
- The ``setup()`` function sets the buzzer pin as an output.
- In the ``loop()`` function:
    - The buzzer is turned on by setting ``BUZZER_PIN`` to ``HIGH``.
    - After waiting 1 second, the buzzer is turned off by setting ``BUZZER_PIN`` to ``LOW``, and then it waits another second.
This will cause the buzzer to alternate between ``ON`` and ``OFF`` every second.

## Write a code for triggering a buzzer when the distance is below a certain threshold using an Arduino board.

```C
#define TRIG_PIN D1
#define ECHO_PIN D2
#define BUZZER_PIN D3

void setup() {
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  Serial.begin(115200);
}

void loop() {
  long duration, distance;

  // Clear the TRIG_PIN
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);

  // Trigger the ultrasonic sensor
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  // Read the echo time
  duration = pulseIn(ECHO_PIN, HIGH);
  distance = (duration * 0.0343) / 2;  // Calculate distance in centimeters

  // Check if the distance is below a certain threshold (e.g., 10 cm)
  if (distance < 10) {
    digitalWrite(BUZZER_PIN, HIGH);  // Turn on the buzzer
  } else {
    digitalWrite(BUZZER_PIN, LOW);  // Turn off the buzzer
  }

  // Print the distance to the Serial Monitor
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  delay(500);  // Wait 500 milliseconds before the next reading
}
```

### Explanation:
- ``TRIG_PIN`` and ``ECHO_PIN``: Used to send and receive the ultrasonic pulses.
- ``BUZZER_PIN``: Pin connected to the buzzer.
- In the ``loop()``:
    - The sensor is triggered to measure the distance.
    - If the distance is less than ``10 cm``, the buzzer is activated by setting the buzzer pin to ``HIGH``.
    - If the distance is ``10 cm`` or more, the buzzer is turned off by setting the pin to ``LOW``.
- This setup will continuously check the distance and trigger the buzzer when an object is detected closer than ``10 cm``.