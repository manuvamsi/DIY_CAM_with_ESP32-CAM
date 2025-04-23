# DIY Camera: Object Detection & Classification with ESP32-CAM

Welcome to the **DIY Camera** project! This badass setup uses the ultra-affordable **ESP32-CAM** module to perform real-time **object detection and classification** using the **Edge Impulse FOMO model** and **TensorFlow Lite**. Whether you’re building a smart security cam, a wildlife tracker, or just geeking out on TinyML, this project is your ticket to low-cost, high-impact machine vision. 🚀

![Circuit Diagram](https://github.com/manuvamsi/DIY_CAM_with_ESP32-CAM/blob/ec8ac67982df2b150eb08ff1252b80440140b75c/DIY%20CAM%20WITH%20ESP32-CAM/Circuit/Ckt_Servo_OLED.png)

## What’s This All About?

The ESP32-CAM is a cheap yet powerful microcontroller with a built-in OV2640 camera and Wi-Fi. Pair it with the **EloquentEsp32cam** library and **Edge Impulse’s FOMO (Faster Objects, More Objects)** model, and you’ve got a lean, mean object-detecting machine. This project walks you through collecting images, training a model, and deploying it to your ESP32-CAM for real-time detection. No PhD in AI required—just some Arduino skills and a bit of hustle. 😎

### Features
- **Real-Time Object Detection**: Spot objects in images or video streams using the FOMO model.
- **Low-Cost Hardware**: ESP32-CAM costs ~$9, making this accessible for hobbyists.
- **Edge Impulse Integration**: Train and deploy ML models with a user-friendly platform.
- **Customizable**: Add servos, OLED displays, or other peripherals (see circuit diagram above).
- **Open-Source**: All code and resources are free to tweak and share.

## Prerequisites

Before you dive in, make sure you’ve got:
- **Hardware**:
  - ESP32-CAM module
  - FTDI programmer (for uploading code)
  - Optional: Servo motor, OLED display (as shown in the circuit)
  - Jumper wires, breadboard, and a 5V power source
- **Software**:
  - [Arduino IDE](https://www.arduino.cc/en/software) (latest version)
  - [Edge Impulse account](https://www.edgeimpulse.com/) (free tier works fine)
  - Web browser (Chrome/Firefox recommended)
- **Libraries**:
  - **EloquentEsp32cam** (v2.7.15 or latest) by Simone Salerno
  - ESP32 board support package (add via Arduino IDE Board Manager)
- **Basic Knowledge**:
  - Arduino programming
  - Wi-Fi setup (2.4 GHz network)
  - Familiarity with image labeling (don’t worry, it’s easy)

## Getting Started

Follow these steps to build your own object-detecting camera. It’s like assembling a Lego set, but with more code and fewer tears.

### Step 1: Set Up the ESP32-CAM
1. **Program the ESP32-CAM**:
   - Connect your ESP32-CAM to an FTDI programmer (check [this guide](https://dronebotworkshop.com/esp32-cam-intro/) for wiring).
   - In Arduino IDE, go to `Tools > Board > ESP32 Wrover Module`.
   - Add the ESP32 board package: `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json` in `File > Preferences > Additional Board Manager URLs`.
   - Install the ESP32 library via `Tools > Board > Board Manager`.
   - Load the `CameraWebServer` example from `File > Examples > ESP32 > Camera` to test your setup. Update `WIFI_SSID` and `WIFI_PASS` with your 2.4 GHz Wi-Fi credentials.
   - Short IO0 to GND, upload the code, then remove the short and press reset.[](https://blog.arducam.com/esp32-machine-vision-learning-guide/)

2. **Verify the Camera**:
   - Open the Serial Monitor (115200 baud) to find the IP address assigned to your ESP32-CAM.
   - Access the IP in a browser (Chrome/Firefox) to view the camera stream. If you see video, you’re golden!

### Step 2: Install the EloquentEsp32cam Library
1. In Arduino IDE, go to `Sketch > Include Library > Manage Libraries`.
2. Search for `EloquentEsp32cam` by Simone Salerno and install version 2.7.15 (or the latest).
3. Open the example sketch: `File > Examples > EloquentEsp32cam > Collect_Images_for_EdgeImpulse`.
4. Update `WIFI_SSID` and `WIFI_PASS` in the code with your Wi-Fi credentials.
5. Upload the sketch to your ESP32-CAM (IO0 to GND during upload, then remove and reset).

### Step 3: Collect Images
1. Open the Serial Monitor to get the local IP address of the ESP32-CAM.
2. Access the IP in a browser to reach the image collection interface.
3. Point the camera at objects you want to detect (e.g., fruits, tools, or your cat).
4. Click to capture images and save them locally.
5. Once you’ve got enough images (50-100 per object class recommended), download the folder.

### Step 4: Train the Model with Edge Impulse
1. Sign up or log in to [Edge Impulse](https://studio.edgeimpulse.com/).
2. Create a new project and select **Bounding Boxes (Object Detection)** with **Espressif ESP-EYE** as the target (closest match to ESP32-CAM).[](https://mlsysbook.ai/contents/labs/seeed/xiao_esp32s3/object_detection/object_detection.html)
3. Go to `Data Acquisition > Upload Data` and upload the folder of images.
4. Label each image in the `Labeling Queue` (draw bounding boxes around objects and assign classes, e.g., “apple” or “banana”).
5. Create an impulse:
   - Add an **Image** processing block.
   - Select the **FOMO (MobileNetV2 0.1)** model for training (optimized for ESP32-CAM).[](https://dronebotworkshop.com/esp32-object-detect/)
6. Train the model (takes a few minutes). Check the confusion matrix for accuracy (aim for >80%).
7. Go to `Deployment`, select **Arduino Library**, and download the `.zip` file.

### Step 5: Deploy the Model
1. In Arduino IDE, go to `Sketch > Include Library > Add .ZIP Library` and import the `.zip` from Edge Impulse.
2. Open the example sketch under `File > Examples > Your_Project_Name > esp32_camera`.
3. Update the camera model and pins (lines 32-75) to match your ESP32-CAM (e.g., `CAMERA_MODEL_AI_THINKER`).[](https://mlsysbook.ai/contents/labs/seeed/xiao_esp32s3/object_detection/object_detection.html)
4. Upload the sketch to your ESP32-CAM (IO0 to GND, then remove and reset).
5. Open the Serial Monitor to see detection results (object names, coordinates, and confidence scores).

### Step 6: Test and Tweak
- Point the camera at your trained objects. The ESP32-CAM will detect and classify them in real time.
- Check the Serial Monitor for output like `Found apple at (x=50, y=50)` or tweak the code to trigger actions (e.g., move a servo or display on an OLED).
- If accuracy sucks, collect more images or adjust the model in Edge Impulse.

## Circuit Diagram
The circuit includes optional components like a servo and OLED for enhanced functionality. Check the diagram below:

![Circuit Diagram](https://github.com/manuvamsi/DIY_CAM_with_ESP32-CAM/blob/ec8ac67982df2b150eb08ff1252b80440140b75c/DIY%20CAM%20WITH%20ESP32-CAM/Circuit/Ckt_Servo_OLED.png)

- **ESP32-CAM**: Core module for camera and processing.
- **Servo**: For physical actions (e.g., pointing at detected objects).
- **OLED**: Displays detection results or status.
- Connect as shown, ensuring proper voltage (5V for ESP32-CAM, 3.3V for OLED).

## Example Use Cases
- **Package Detection**
- **Detected object will be classified**
- **Inventory Maintanence**.


## Troubleshooting
- **No IP Address?** Check Wi-Fi credentials and ensure you’re on a 2.4 GHz network. Reset the ESP32-CAM.
- **Upload Fails?** Double-check IO0 is shorted to GND during upload. Press reset if you see dots/dashes.[](https://how2electronics.com/esp32-cam-based-object-detection-identification-with-opencv/)
- **Low Accuracy?** Collect more diverse images (different angles, lighting) or retrain the model.
- **Camera Not Working?** Verify the camera model in the code (e.g., `CAMERA_MODEL_AI_THINKER`) and check connections.

## Resources
- [EloquentEsp32cam Library](https://github.com/eloquentarduino/EloquentEsp32cam)[](https://github.com/eloquentarduino/EloquentEsp32cam)
- [Edge Impulse Documentation](https://docs.edgeimpulse.com/docs)
- [ESP32-CAM Guide by DroneBot Workshop](https://dronebotworkshop.com/esp32-cam-intro/)[](https://blog.arducam.com/esp32-machine-vision-learning-guide/)
- [FOMO Tutorial by Eloquent Arduino](https://eloquentarduino.com/esp32-cam-object-detection)[](https://eloquentarduino.com/posts/esp32-cam-object-detection)
- [YouTube: Simple ESP32-CAM Object Detection](https://www.youtube.com/watch?v=your-video-link)[](https://www.youtube.com/watch?v=HDRvZ_BYd08)

## Contributing
Got ideas to make this project even cooler? Fork the repo, tweak the code, and submit a pull request! Want to add new features like MQTT integration or a web dashboard? Let’s collab. 😈

## License
This project is licensed under the [MIT License](LICENSE). Hack it, share it, just give a shoutout to the original repo.

## Acknowledgments
- **Simone Salerno** for the EloquentEsp32cam library.
- **Edge Impulse** for making TinyML accessible.
- **ESP32 Community** for endless tutorials and support.
- **You**, for building awesome shit with this guide!

Happy hacking, you glorious maker! If you build something epic, share it with me on [GitHub Discussions](https://github.com/manuvamsi/DIY_CAM_with_ESP32-CAM/discussions). 🚀
