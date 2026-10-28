# iot-fake-number-plate-detection
IoT Enabled Fake Number Plate Detection

A system that detects fake or tampered number plates by comparing the plate
read by a camera with the RFID tag fixed to the vehicle, and indicates a fine
when the two do not match.

> Academic project, Rajagiri School of Engineering & Technology (RSET), Kochi.
> Team: Sayujya Krishna, Vishnupriya, Shreya.

## Problem
A number plate alone is easy to forge or swap. If a vehicle is identified only
by its plate, a fake plate can let it avoid fines and tracking.

## Solution
Each vehicle carries an RFID tag linked to its registered plate number. At a
checkpoint, a camera reads the visible plate and an RFID reader reads the tag.
If the plate text does not match the registered number, the vehicle is flagged.

## How It Works
1. A webcam captures the vehicle at the checkpoint.
2. **YOLOv8** detects the number plate region. ✏️
3. OCR extracts the plate characters from the detected region. ✏️
4. The RFID reader reads the vehicle's tag ID.
5. The tag ID is looked up in a database to get the registered plate number.
6. The detected plate is compared with the registered one.
7. **Match:** vehicle passes. **Mismatch:** it is flagged as a fake plate, a
   fine indication is raised (buzzer + LED + alert), and the event is logged
   over IoT. ✏️

## Hardware & Software
- **Detection:** YOLOv8 (Ultralytics) trained on a number plate dataset ✏️
- **OCR:** EasyOCR ✏️
- **Language:** Python 3
- **Tag system:** RFID reader (RC522) with 13.56 MHz tags ✏️
- **Controller / IoT:** NodeMCU for alerts and cloud upload ✏️
- **Output:** Buzzer and LEDs for fine indication

## Example Output
```
[INFO] Plate detected: KL07AB1234 (confidence 0.91)
[INFO] RFID tag read: 4F 2A 9C 11
[INFO] Registered plate for tag: KL07AB1234
[RESULT] MATCH - vehicle verified
```
```
[INFO] Plate detected: KL07XY9999 (confidence 0.88)
[INFO] RFID tag read: 4F 2A 9C 11
[INFO] Registered plate for tag: KL07AB1234
[RESULT] MISMATCH - FAKE PLATE SUSPECTED - fine indicated
```

## Results
✏️ Replace these with your measured values.

| Metric | Value |
|--------|-------|
| Plate detection accuracy (YOLOv8) | ~94% |
| Character recognition accuracy | ~88% |
| RFID read success at checkpoint | ~98% |
| Correct mismatch detection in test cases | 9 of 10 |

Accuracy dropped in low light and with motion blur, which affected the plate
reading more than the RFID check.

## My Role ✏️
- Integrated the RFID reader and the matching logic
- Worked on the YOLOv8 detection pipeline and testing
- Built the alert circuit (buzzer + LEDs) and IoT upload

## Repository Structure
```
├── README.md
├── code/        # detection, OCR and RFID matching scripts
├── model/       # trained YOLOv8 weights (best.pt)
├── docs/        # project report, presentation
├── images/      # circuit diagram, detection screenshots
└── demo/        # demo video or link
```

## Limitations & Future Work
- Improve plate reading in low light with better preprocessing or an IR camera.
- Replace the local lookup with a secure vehicle-registration database.
- Handle multiple vehicles in one frame and run in real time.
- RFID tags can be cloned, so add encrypted tags
