# ESP32-C3 Wireless Air Mouse

This is the whole codebase for my **wireless Air Mouse**. It has the code, CAD files and circuit diagram needed to make your own Air Mouse using an **ESP32-C3 Super Mini and MPU6050**.

The main reason I built this was because I wanted to make a mouse which doesn't actually need to be moved on a table. The MPU6050 detects the rotation of the controller and converts it into mouse movement over **Bluetooth HID**. I also added two buttons for left and right click, and holding the buttons gives scrolling functionality.

It is basically a small **motion-controlled wireless mouse** which can be used with a laptop or PC without any receiver or Wi-Fi.

## Features

- Wireless mouse using Bluetooth HID
- Motion-based cursor control using MPU6050
- ESP32-C3 Super Mini as the main controller
- Left and right mouse buttons
- Hold buttons for scrolling
- Adjustable cursor sensitivity
- Adjustable smoothing
- Gyroscope calibration on startup
- No Wi-Fi or external receiver required
- Battery powered
- Compact 3D printable design

## BOM – Bill of Materials

| Component | Quantity | Price | Purpose | Purchase Link |
|-----------|:--------:|:-----:|---------|:-------------:|
| ESP32-C3 Super Mini | 1 | — | Main controller and Bluetooth HID | [Buy Here](https://robocraze.com/products/esp32-c3-mini-development-board-unsoldered?variant=48465411506400&country=IN&currency=INR) |
| MPU6050 | 1 | — | Gyroscope and motion sensing | [Buy Here](https://robocraze.com/products/mpu-6050-triple-axis-accelerometer-gyroscope-module) |
| TP4056 Type-C Module | 1 | — | Li-ion battery charging and protection | [Buy Here](https://robocraze.com/products/tp4056-battery-charger-c-type-module-with-protection-1) |
| 3.7V 2500mAh 18650 Li-ion Battery | 1 | — | Power source | [Buy Here](https://robocraze.com/products/3-7v-2500mah-18650-li-ion-battery) |
| 18650 Single Cell Holder | 1 | — | Battery mounting | [Buy Here](https://robocraze.com/products/18650-1-cell-holder) |
| Tactile Push Button | 2 | — | Left and right mouse buttons | [Buy Here](https://www.amazon.in/Tactile-momentry-button-Switch-Button/dp/B07NCZJX3M) |
| 3D Printed Parts | As required | Self-manufactured | Air Mouse body and structural parts | Self Printed |

## Circuit Diagram

I have added the complete circuit diagram below. You can use it as a reference while wiring your own Air Mouse.

<img width="1000" alt="Air Mouse Circuit Diagram" src="YOUR_IMAGE_LINK_HERE" />

### Main Connections

**MPU6050 → ESP32-C3**

| MPU6050 | ESP32-C3 |
|---------|----------|
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO 5 |
| SCL | GPIO 4 |

**Buttons**

| Button | ESP32-C3 |
|--------|----------|
| Left Button | GPIO 3 |
| Right Button | GPIO 2 |
| Other side of both buttons | GND |

The buttons use the ESP32's internal pull-up resistors, so no external resistors are required.

## How to Build this...

I have uploaded all the needed code, CAD models and circuit diagram in my repo.

**step 1:** First get all the components mentioned in the BOM. The main parts required are the ESP32-C3 Super Mini, MPU6050, two tactile buttons, TP4056 and a single 18650 battery.

**step 2:** Connect the MPU6050 to the ESP32-C3. Connect SDA to GPIO 5 and SCL to GPIO 4. Connect VCC to 3.3V and GND to GND.

**step 3:** Connect the two tactile buttons. One button goes to GPIO 3 and the other goes to GPIO 2. Connect the other side of both buttons to GND.

**step 4:** Connect the 18650 battery to the TP4056 battery terminals. The TP4056 is used for charging the single-cell Li-ion battery.

**step 5:** Upload the Air Mouse code to the ESP32-C3 Super Mini. In Arduino IDE select **ESP32C3 Dev Module** as the board.

**step 6:** Once the ESP32 starts, keep the Air Mouse still during startup. The code takes several readings from the MPU6050 and calculates the gyro bias automatically. This is important because otherwise the cursor can slowly move even when you are not moving the Air Mouse.

**step 7:** Open the Bluetooth settings on your laptop/PC and connect to the device named:

```text
AIRMOUSE
```

Once connected, rotating the Air Mouse will control the cursor.

**step 8:** The buttons work like this:

```text
Short press Left  → Left click
Short press Right → Right click

Hold Left  → Scroll down
Hold Right → Scroll up
```

**step 9:** If the cursor direction feels wrong for your particular physical orientation of the MPU6050, you can change the signs in the movement calculation. In my current setup I have inverted the X axis:

```cpp
float moveXf = -(fX * sensitivity);
float moveYf =  (fY * sensitivity);
```

**step 10:** Finally, assemble everything inside the 3D printed body. The exact placement of the MPU6050 matters because its orientation determines how the Air Mouse responds to your hand movements.

## Known Issues

1. The cursor direction depends on the physical orientation of the MPU6050.
2. The MPU6050 needs to remain still during startup calibration.
3. Different MPU6050 modules can have slightly different gyro drift.
4. Cursor sensitivity may need to be adjusted depending on the user.
5. The battery voltage should be handled properly through the charging/power circuit.
6. Bluetooth HID compatibility can vary slightly between operating systems.
7. Holding the buttons is currently programmed for scrolling rather than continuous mouse-button holding.
8. Very small movements can be affected by the configured deadzone.

## CAD Models

<img width="1000" alt="Air Mouse CAD Model" src="YOUR_IMAGE_LINK_HERE" />

<img width="1000" alt="Air Mouse CAD Assembly" src="YOUR_IMAGE_LINK_HERE" />

The CAD files are included in the repository so that the body can be modified or 3D printed directly.

## Code

The main code uses:

- `Wire.h` for I²C communication with the MPU6050
- `MPU6050.h` for reading the gyroscope
- `BleMouse.h` for Bluetooth HID mouse functionality

The main parameters can be adjusted directly in the code:

```cpp
float smoothing   = 0.82;
float sensitivity = 0.20;
int deadzone      = 1;
```

So if the cursor is too slow, increase `sensitivity`. If the cursor feels too shaky, increase `smoothing`.

The whole idea is pretty simple but I think it makes a normal mouse quite interesting — **instead of moving the mouse, you move the mouse itself.**
