# Plant HP Bar

Built in 24 hours at **HenHacks 2026** (University of Delaware).

Plant HP Bar turns plant care into a game. Sensors track a plant's soil moisture, air temperature, and humidity, and an LCD shows the plant's health as a video-game-style HP bar that rises when conditions are good and drains when they aren't.

## How it works

1. **Name your plant.** On startup, use the joystick to enter a name of up to 10 characters. Move left and right to pick a position, up and down to change the letter, and press the button to confirm.
2. **Watch its HP.** Every 2 seconds the Arduino reads the sensors and updates the display with `<name>'s HP:` above a 14-segment health bar.

HP works like health in a game, not a fresh score each reading. It starts at 100 and changes by up to ±1 per cycle, based on how close each reading is to the plant's ideal range:

| Factor | Weight | Ideal range |
|---|---|---|
| Soil moisture | 50% | 300–700 (sensor reading, best near 500) |
| Temperature | 30% | 13–27 °C (best near 20 °C) |
| Air humidity | 20% | 40–70% (best near 55%) |

## Hardware

- Arduino Uno
- DHT11 temperature and humidity sensor
- Soil moisture sensor (analog)
- 16x2 character LCD (HD44780-compatible)
- Analog joystick with push button

### Pin connections

| Component | Arduino pin(s) |
|---|---|
| LCD RS, EN | 11, 12 |
| LCD D4, D5, D6, D7 | 6, 5, 4, 3 |
| DHT11 data | 2 |
| Soil moisture sensor | A0 |
| Joystick X, Y, button | A1, A2, A3 |

### Wiring diagram

<img width="3000" height="4506" alt="Wiring diagram for Plant HP Bar" src="https://github.com/user-attachments/assets/984861e2-f890-4508-b5be-0f775820ae39" />

## Code structure

```
main/
├── main.ino               Main loop, name-entry menu, and HP updates
├── graphics.cpp/.h        LCD output: text, health bar, character selection
├── soil_hydrometer.cpp/.h Sensor reads and the health-scoring model
└── joystick.cpp/.h        Joystick and button input
monitor/
└── monitor.py             Optional Python serial monitor for debugging
```

## Running it

1. Install the [Arduino IDE](https://www.arduino.cc/en/software).
2. In the Library Manager, install **DHT sensor library** and **Adafruit Unified Sensor** (both by Adafruit). LiquidCrystal comes with the IDE.
3. Open `main/main.ino`, select **Arduino Uno** as the board, and upload.

To watch the HP values over serial without the Arduino IDE, run the monitor at 9600 baud. Change `PORT` in the script to match your system (for example `COM3` on Windows).

```bash
pip install pyserial
python monitor/monitor.py
```

## Team

| Member | Contributions |
|---|---|
| Anthony Imperiale ([@AntI300000](https://github.com/AntI300000)) | Modular C++ code structure, LCD graphics and health bar, joystick name-entry menu, documentation, wiring |
| Jake Brennan ([@jake-brennan714](https://github.com/jake-brennan714)) | Sensor integration, health-scoring model, serial monitor, wiring |
| Daniel La Mastra ([@dal1998](https://github.com/dal1998)) | LCD output and joystick input |
