Amir Gerald G. Gilbuena
Submission for Embedded Systems TASK PERFORMANCE 05 PREFINALS

Link for the video simulation: 

https://youtu.be/DXoMR3csS-o


Link for the tinkercad file:

https://www.tinkercad.com/things/gZUuDGiKr2N-exquisite-blorr/editel?returnTo=https%3A%2F%2Fwww.tinkercad.com%2Fdashboard%2Fdesigns%2Fall&sharecode=iEKp99Jq1iZKicKEc32m0-4bYuvvBrUGAssYLNSBXwM



Can't upload code since it's in tinkercad so i'll drop it here.
#include <IRremote.h>

const int IR_PIN = 11;

const int ledRelay[4] = {2, 3, 4, 5};
const int PUMP_RELAY   = 6;
const int WASHER_RELAY = 7;

const uint8_t BTN_1 = 16;
const uint8_t BTN_2 = 17;
const uint8_t BTN_3 = 18;
const uint8_t BTN_4 = 20;

bool isOn[4] = {false, false, false, false};

void toggleChannel(int ch) {
  isOn[ch] = !isOn[ch];
  digitalWrite(ledRelay[ch], isOn[ch] ? HIGH : LOW);

  if (ch == 2) digitalWrite(PUMP_RELAY,   isOn[ch] ? HIGH : LOW);
  if (ch == 3) digitalWrite(WASHER_RELAY, isOn[ch] ? HIGH : LOW);

  Serial.print("Channel ");
  Serial.print(ch + 1);
  Serial.println(isOn[ch] ? " ON" : " OFF");
}

void setup() {
  Serial.begin(9600);
  IrReceiver.begin(IR_PIN);

  for (int i = 0; i < 4; i++) {
    pinMode(ledRelay[i], OUTPUT);
    digitalWrite(ledRelay[i], LOW);
  }
  pinMode(PUMP_RELAY, OUTPUT);
  pinMode(WASHER_RELAY, OUTPUT);
  digitalWrite(PUMP_RELAY, LOW);
  digitalWrite(WASHER_RELAY, LOW);
}

void loop() {
  if (IrReceiver.decode()) {
    uint8_t cmd = IrReceiver.decodedIRData.command;
    Serial.print("Received command: ");
    Serial.println(cmd);

    switch (cmd) {
      case BTN_1: toggleChannel(0); break;
      case BTN_2: toggleChannel(1); break;
      case BTN_3: toggleChannel(2); break;
      case BTN_4: toggleChannel(3); break;
      default: break;
    }

    IrReceiver.resume();
  }
}
