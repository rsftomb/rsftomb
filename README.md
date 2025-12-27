📓WarPi.G – Patch Notes
Initial Release (Boar# 1225.01 Tusk) Beta

Features

    Wi-Fi and Bluetooth scanning with live telemetry
    Animated radar visualization with sweep-based detection
    Signal-strength–based positioning and blip aging
    Real-time counts for current and total devices
    Mobile-friendly WebUI with Dashboard, Wi-Fi, and Bluetooth tabs

System & Stability

    Reliable startup via systemd with boot-time safeguards
    Stable background scanning and safe restarts
    SD card health and read-only filesystem detection

UI & UX

    Green-on-black OLED/WebUI aesthetic
    Scrollable live device lists
    Consistent icon + metric layout
    1-second real-time refresh


📦 **Hardware Requirements**

Raspberry Pi Zero 2 W

Two SSD1306 OLED displays

Working Wi-Fi + Bluetooth (built-in)

Software

**Install dependencies:**

sudo apt update
sudo apt install python3-pip python3-smbus python3-pil i2c-tools
pip3 install luma.oled psutil


**Enable I2C:**

sudo raspi-config

🔧 How It Works
Thread & Data Flow
Component	Description
wifi_scanner()	Performs repeated iwlist wlan0 scan operations. Updates count + unique total.
bt_scanner()	Runs bluetoothctl scan on/off. Tracks visible and total MACs.
get_cpu_temp()	Converts Pi temp to Fahrenheit.
get_usb_voltage()	Reads Pi 5V rail via vcgencmd measure_volts.
canvas(oled)	Draws each frame on both displays.
threading.Lock()	Prevents races between the two scan threads.


▶ Setup AutoRun Automatically (systemd)

Create a service:

/etc/systemd/system/oled.service

[Unit]
Description=WarScanner OLED Display Service
After=network.target bluetooth.target

[Service]
ExecStart=/usr/bin/python3 /home/YOURUSER/oled.py
Restart=always
User=pi

[Install]
WantedBy=multi-user.target


Enable and start:

sudo systemctl enable oled.service
sudo systemctl start oled.service


Restart live without reboot:

sudo systemctl restart oled.service

🧪 Known Limitations

Wi-Fi scan speed depends on local RF noise.

Bluetooth scanning may miss some devices using privacy MAC rotation.

OLEDs can burn in if static text is displayed for long periods.

🚀 Planned for V2

Auto-save CSV of all seen Wi-Fi + BT devices

Hotkey-triggered mode switching

Optional e-ink version (ultra-low-power)

📜 License (MIT)
MIT License

Copyright (c) 2025 
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.

💻 Developed by:
Jon Leary aka jleary53 / Sunshine State Media LLC


<!---
rsftomb/rsftomb is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
