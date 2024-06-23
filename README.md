<div align="center">
    <img src="https://github.com/Diksha2220/Media/blob/main/communication.gif" alt="Caesar Cipher" width="500"/>
</div>

### Project Idea: 🌊🔦 Underwater Wireless Communication System using Arduino Uno and IR LEDs

#### Objective
🔧 Build a simple underwater wireless communication system using Arduino Uno and infrared LEDs to transmit data between two Arduino boards. This project demonstrates optical communication principles in a controlled underwater environment.

#### Components Needed
- 🛠️ Two Arduino Uno boards
- 🌟 IR LED and photodiode pairs (one pair for each Arduino)
- ⚡ Resistors (220 ohms for LEDs, 10k ohms for pull-down resistors)
- 🍞 Breadboards
- 🔗 Jumper wires
- 📡 IR LED and photodiode pairs (one pair for each Arduino)
- 💧 Waterproof enclosure (to protect Arduino and components)
- 🐠 Clear water tank or container

#### Circuit Setup

**Transmitter (Arduino 1) Connections:**
- Connect the IR LED anode to digital pin 7 through a 220-ohm resistor.
- Connect the IR LED cathode to ground.
- Connect the three slide switches to digital pins 2, 3, and 4 respectively.
- Connect one terminal of each slide switch to ground.
- Connect the other terminal of each slide switch to the corresponding digital pin.

**Receiver (Arduino 2) Connections:**
- Connect the red LED anode to digital pin 5 through a 220-ohm resistor.
- Connect the green LED anode to digital pin 6 through a 220-ohm resistor.
- Connect the yellow LED anode to digital pin 9 through a 220-ohm resistor.
- Connect the cathodes of all LEDs to ground.
- Connect the photodiode anode to analog pin A0.
- Connect the photodiode cathode to ground through a 10k-ohm resistor.
- Connect the photodiode anode to 5V through a 220-ohm resistor.

### Arduino Sketch (Code)

#### Transmitter Code (Arduino 1)

```cpp
const int switch1_pin = 2;
const int switch2_pin = 3;
const int switch3_pin = 4;
const int ir_led_pin = 7;

void setup() {
  pinMode(switch1_pin, INPUT_PULLUP);
  pinMode(switch2_pin, INPUT_PULLUP);
  pinMode(switch3_pin, INPUT_PULLUP);
  pinMode(ir_led_pin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int switch1_state = digitalRead(switch1_pin);
  int switch2_state = digitalRead(switch2_pin);
  int switch3_state = digitalRead(switch3_pin);

  char data = (switch1_state == LOW ? '1' : '0') |
              (switch2_state == LOW ? '2' : '0') |
              (switch3_state == LOW ? '4' : '0');

  sendData(data);
  delay(200);  // Adjust delay for communication speed
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
const int red_led_pin = 5;
const int green_led_pin = 6;
const int yellow_led_pin = 9;
const int photodiode_pin = A0;
const int threshold = 512;  // Set an appropriate threshold for detecting IR light

void setup() {
  pinMode(red_led_pin, OUTPUT);
  pinMode(green_led_pin, OUTPUT);
  pinMode(yellow_led_pin, OUTPUT);
  pinMode(photodiode_pin, INPUT);
  Serial.begin(9600);
}

void loop() {
  char receivedData = receiveData();
  if (receivedData != '\0') {
    updateLEDs(receivedData);
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

void updateLEDs(char data) {
  digitalWrite(red_led_pin, bitRead(data, 0) ? HIGH : LOW);
  digitalWrite(green_led_pin, bitRead(data, 1) ? HIGH : LOW);
  digitalWrite(yellow_led_pin, bitRead(data, 2) ? HIGH : LOW);
}
```

### How It Works
1. **Transmitter**: The first Arduino (transmitter) reads the states of three slide switches. It creates a binary representation of the switch states and sends this data bit by bit using the IR LED.
2. **Receiver**: The second Arduino (receiver) uses a photodiode to detect the IR light. It reads the incoming bits, reconstructs the data, and updates the states of the red, green, and yellow LEDs accordingly.
3. **Communication**: This setup assumes a simple protocol where each bit is sent in sequence with a fixed delay. Adjust the delay to match the communication speed and the distance between the transmitter and receiver.

### Usage
1. Upload the transmitter code to the first Arduino.
2. Upload the receiver code to the second Arduino.
3. Place both Arduino boards in a waterproof enclosure if necessary.
4. Ensure the IR LED and photodiode are aligned for effective communication.
5. Slide the switches on the transmitter Arduino. The corresponding LEDs on the receiver Arduino should light up based on the switch positions.

### Important Considerations
IR Light Limitations: 🌊 Infrared light can be affected by environmental conditions, including water and obstacles. Ensure a clear line of sight for optimal communication.
Synchronization: ⏳ Ensure the timing (delays) in both transmitter and receiver are perfectly matched for accurate communication.
Waterproofing: 🛡️ Make sure all electronic components are properly enclosed if used in environments where they might get wet.

📹 [View practical project working file ](https://github.com/Diksha2220/Media/blob/main/UWWCS%20Practical.mp4))

<div align="center">
    <img src="https://github.com/Diksha2220/Media/blob/main/UWWCS%20led.gif" alt="Caesar Cipher" width="500"/>
</div>
