# esp32-security-cam

An ESP32-CAM security camera sketch. It streams live video from the camera over Wi-Fi as an MJPEG stream and lights an LED for a few seconds when a door sensor opens.

## Features

* Live MJPEG video stream at HD resolution (1280×720), served on port 80.
* Door sensor input (magnetic reed switch) on GPIO 13.
* LED on GPIO 4 lights at 50 % brightness for 3 seconds when the door opens.
* Pin map for the **AI-Thinker ESP32-CAM** board.

## Hardware

* An AI-Thinker ESP32-CAM board (the pin map in the sketch is for this model; other models need their own pin definitions).
* A magnetic door (reed) switch between **GPIO 13** and GND. The pin uses the internal pull-up, so the input reads HIGH when the switch opens.
* An LED on **GPIO 4**. On the AI-Thinker board this is the built-in flash LED.
* A 5 V power supply that can handle the camera's current draw. The sketch disables the brownout detector, which hides supply problems instead of fixing them.
* A USB-to-serial adapter for flashing.

## Software

* [Arduino IDE](https://www.arduino.cc/en/software) with ESP32 board support.
* The sketch uses `ledcSetup()` / `ledcAttachPin()`, which were removed in version 3.x of the Arduino ESP32 core ([migration guide](https://github.com/espressif/arduino-esp32/blob/master/docs/en/migration_guides/2.x_to_3.0.rst)), so it needs a 2.x core.

## Configuration

Edit these values in [`ESP32-Sec-Cam/ESP32-Sec-Cam.ino`](ESP32-Sec-Cam/ESP32-Sec-Cam.ino) before uploading:

| Setting | Default | Description |
|---------|---------|-------------|
| `ssid` | *(empty)* | Wi-Fi network name |
| `password` | *(empty)* | Wi-Fi password |
| `DOOR_SENSOR_PIN` | `13` | GPIO for the door switch |
| `LED_PIN` | `4` | GPIO for the LED |

## Usage

1. Fill in your Wi-Fi credentials.
2. Connect GPIO 0 to GND and press RESET to enter flash mode, select the **AI Thinker ESP32-CAM** board, then upload. Disconnect GPIO 0 from GND and press RESET again to run the sketch.
3. Open the serial monitor at **115200** baud. After Wi-Fi connects, the sketch prints `Camera Stream Ready! Go to: http://<ip>`.
4. Open `http://<ip>/` in a browser to watch the stream. Only one client can watch at a time; a second viewer's request waits until the first disconnects.

## Known limitations

* **No authentication.** Anyone on the network can open the stream.
* **Door events only light the LED.** They are not sent anywhere and are not linked to the stream.
* The sketch waits for Wi-Fi forever at startup and has no reconnect logic of its own; it relies on the ESP32 core's default auto-reconnect. The IP address may change after a reconnect.

## Credits

The camera setup and streaming code is based on the Random Nerd Tutorials example [ESP32-CAM Video Streaming Web Server](https://randomnerdtutorials.com/esp32-cam-video-streaming-web-server-camera-home-assistant/) by Rui Santos.

## License

[Apache License 2.0](LICENSE)
