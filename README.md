### Project Idea: 🌐🔵 Underwater Wireless Communication System using Arduino Uno and IR LEDs

#### Objective
🔧 Develop a simple underwater wireless communication system using Arduino Uno and infrared LEDs to transmit data between two Arduino boards. This project illustrates the principles of optical communication in a controlled underwater environment.

#### Components Needed
- 🛠️ Two Arduino Uno boards
- 🌟 IR LED and photodiode pairs (one pair for each Arduino)
- ⚡ Resistors (220 ohms for IR LEDs)
- 🍞 Breadboards
- 🔗 Jumper wires
- 💧 Waterproof enclosure (to protect Arduino and components)
- 🐠 Clear water tank or container

#### Circuit Setup

Transmitter (Arduino 1) Connections:
Connect the IR LED anode to digital pin 3 through a 220-ohm resistor.
Connect the IR LED cathode to ground.

Receiver (Arduino 2) Connections:
Connect the photodiode anode to analog pin A0.
Connect the photodiode cathode to ground through a 10k-ohm resistor.
Connect the photodiode anode to 5V through a 220-ohm resistor.

### Arduino Sketch (Code)

#### Transmitter Code (Arduino 1)

```cpp
const int ir_led_pin = 3;

void setup() {
  pinMode(ir_led_pin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  if (Serial.available() > 0) {
    char data = Serial.read();
    sendData(data);
  }
}

void sendData(char data) {
  for (int i = 0; i < 8; i++) {
    if (bitRead(data, i)) {
      digitalWrite(ir_led_pin, HIGH);  // Send '1'
    } else {
      digitalWrite(ir_led_pin, LOW);  // Send '0'
    }
    delay(100);  // Adjust delay for baud rate
  }
  digitalWrite(ir_led_pin, LOW);  // Ensure LED is off after transmission
}
```

#### Receiver Code (Arduino 2)

```cpp
const int photodiode_pin = A0;
const int threshold = 512;  // Set an appropriate threshold for detecting IR light

void setup() {
  pinMode(photodiode_pin, INPUT);
  Serial.begin(9600);
}

void loop() {
  char receivedData = receiveData();
  if (receivedData != '\0') {
    Serial.print("📩 Received: ");
    Serial.println(receivedData);
  }
}

char receiveData() {
  char data = 0;
  for (int i = 0; i < 8; i++) {
    int sensor_value = analogRead(photodiode_pin);
    if (sensor_value > threshold) {
      bitWrite(data, i, 1);
    } else {
      bitWrite(data, i, 0);
    }
    delay(100);  // Adjust delay to match the transmitter
  }
  return data;
}
```

### How It Works
1. **Transmitter**: The first Arduino (transmitter) reads serial input data from the serial monitor. It converts each character into its binary representation and sends it bit by bit using the IR LED.
2. **Receiver**: The second Arduino (receiver) uses a photodiode to detect the IR light. It reads the incoming bits and reconstructs the original character, then prints it to the serial monitor.
3. **Communication**: This setup assumes a simple protocol where each bit is sent in sequence with a fixed delay. Adjust the delay to match the communication speed and the distance between transmitter and receiver.

### Usage
1. Upload the transmitter code to the first Arduino.
2. Upload the receiver code to the second Arduino.
3. Place both Arduino boards in a waterproof enclosure if necessary.
4. Submerge both Arduinos in clear water within a short distance.
5. Open the serial monitor for both Arduinos.
6. Type a message in the transmitter’s serial monitor. The receiver should detect and display the transmitted message.

### Important Considerations
IR Light Limitations: 🌊 Infrared light is absorbed quickly in water, limiting the effective communication distance. This setup works best in very clear water and short distances.
Synchronization: ⏳ Ensure the timing (delays) in both transmitter and receiver are perfectly matched for accurate communication.
Waterproofing: 🛡️ Make sure all electronic components are properly enclosed to prevent water damage.

This project showcases the concept of underwater communication with IR light. For practical applications, exploring acoustic or ultrasonic communication methods is recommended due to their superior performance in underwater environments.
