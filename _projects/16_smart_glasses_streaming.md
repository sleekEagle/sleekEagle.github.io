---
layout: page
title: Data streaming tools for smart glasses
description: Two open-source Android apps with Python clients that stream camera frames and motion sensor data from smart glasses to a computer.
importance: 12
category: research
---

Collecting research data from smart glasses means getting camera and sensor streams off the device and onto a computer,
with timestamps you can trust. I built two open-source Android apps, each with a Python client, to do that.

**[camstrm](https://github.com/sleekEagle/camstrm)** streams camera frames over Wi-Fi via TCP.

- Supports autofocus, fixed-focus and focal-stack video capture.
- Frames are displayed live on the computer and saved as video or as timestamped images.

**[sensor_stream](https://github.com/sleekEagle/sensor_stream)** streams motion sensor data over USB or Wi-Fi via ADB.

- Logs both device and computer timestamps, so the sensor data can be synchronised with other streams.

- Code: [sleekEagle/camstrm](https://github.com/sleekEagle/camstrm) and [sleekEagle/sensor_stream](https://github.com/sleekEagle/sensor_stream)
