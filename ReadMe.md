# RaspberryPi 4
In this repository, we will provide some guidelines for setting up RaspberryPi4.
## Set Up the firmware
To start using RaspberryPi4 you need a SD card to burn RaspberryPi OS. For burning OS, you can use RaspberryPi4 Imager; it can be downloaded from [This Link](https://www.raspberrypi.com/software/).
RaspberryPi Imager look like picture below:

![RP Imager](https://github.com/Sharif-Smart-and-Secure-Edge-Cloud-Lab/RaspberryPi4/blob/M.Amin.H.Khodaverdian/RP%20Imager.JPG)

You can choose your arbitrary OS and select your SD card, then click on Write to burn OS to your SD card. After putting your SD Card in RaspberryPi, it will use this OS.

### SSH
For connecting to your RaspberryPi, one of the most common ways is using SSH(Secure Shell Protocol). But by default, SSH is disabled in RaspberryPi.In the new version of RaspberryPi Imager, you can enable SSH.

There is another way to make a file with the **ssh** name and copy the file into the SD card. It will enable ssh automatically in your RaspberryPi

## Drive Wi-Fi
To connect your RaspberryPi to an arbitrary Wi-Fi, there is two way that we will describe it.
### First Way
In the latest version of RaspberryPi imager, you can choose a Wi-Fi that will be the default Wi-Fi for RaspberryPi. You can see the settings in the image below:

![WIFI](https://github.com/Sharif-Smart-and-Secure-Edge-Cloud-Lab/RaspberryPi4/blob/M.Amin.H.Khodaverdian/Wi-Fi.JPG)

### Second Way
Put the Raspberry Pi OS SD card into your computer. Navigate to the boot directory. Then add your **wpa_supplicant.conf** file. The **wpa_supplicant.conf** file should contain this commands:
```
country=IR # Your 2-digit country code
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
network={
    ssid="YOUR_NETWORK_NAME"
    psk="YOUR_PASSWORD"
    key_mgmt=WPA-PSK
}
```

Put your SD card in the Raspberry Pi, boot, and connect.

## Drive BLE
For start using Bluetooth you need to do following steps.(I confront several problems after using default BLE so I search and the results([best result link](https://www.instructables.com/Control-Bluetooth-LE-Devices-From-A-Raspberry-Pi/)) are written below)

By default, the Raspbian distribution comes without a Bluetooth stack. The bluez package is quite old and has patchy support for Low Energy. You can build and install a more modern version as described below.

After the system is up and running open up the Terminal program and a browser window, then start following the commands.

**Do Not do this:** 
```
sudo apt-get install bluez
```

In case you have it already installed, go ahead and remove it. If you're not sure if you have it installed, go ahead and do this step anyway:
```
sudo apt-get --purge remove bluez
```
Next, we have to determine what's the latest version available. To do this, navigate to [the official website](https://www.kernel.org/pub/linux/bluetooth/) and look for the package bluez-X.XX.tar.xz where X.XX is the version

Then, go back to the Terminal on the Raspberry Pi and remembering to change X.XX for the latest version we find we enter:
```
cd ~; wget https://www.kernel.org/pub/linux/bluetooth/bluez-X.XX.tar.xz
```

Subsequently, we uncompress the package by:
```
tar xvf bluez-X.XX.tar.xz
```

We need at this point to make sure all the necessary libraries for running the bluetooth stack:

```
sudo apt-get install libusb-dev libdbus-1-dev libglib2.0-dev libudev-dev libical-dev libreadline-dev
```

Are now ready to compile the bluez package:

```
cd bluez-X.XX

export LDFLAGS=-lrt

./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var --enable-library -disable-systemd

make

sudo make install
```

For a strange reason the standard installation process misses installing one of the files to the correct directory. To solve this:

```
sudo cp attrib/gatttool /usr/bin/
```
### Using BLE
You can use a monitor or VNCViewer to use BLE or commands. This section will give a guide to using BLE with commands.

#### Checking Bluetooth Status
Before you can add Bluetooth devices, the Bluetooth service on your computer must be up and running. You can check it with the help of the **systemctl** command.

```
sudo systemctl status bluetooth
```

If the Bluetooth service status is not active you will have to enable it first. Then start the service so it launches automatically whenever you boot your computer.

```
sudo systemctl enable bluetooth
sudo systemctl start bluetooth
```

#### Scanning for Nearby Devices
To actively search for Bluetooth devices that you can connect to, use the **scan** command as follows:

```
bluetoothctl scan on
```

To make your Bluetooth adapter discoverable to other devices, use the following command:

```
bluetoothctl discoverable on
```

#### Connecting to Your Device
Now that you have a list of Bluetooth devices you can connect to, use the MAC address to connect to a particular device.
The simplest way to connect with a Bluetooth device is to pair it with your RaspberryPi using the pair command.

```
bluetoothctl pair $MAC_address
```

If the device you are connecting to has a GUI interface, for example, a smartphone, the device will display a prompt asking you to accept the connection. The system will also ask you to confirm the pairing on your PC. You can do so by typing yes in the command line.

For devices that are already paired with your PC, you can simply connect to them in the future using the connect command as follows:

```
bluetoothctl connect $MAC_address
```

#### Listing Paired Devices With bluetoothctl
You can look at the devices that are currently paired with your system by running the following command:

```
bluetoothctl paired-devices
```

## Set up the Camera
First of all, we should connect Camera to RaspberryPi carefully as image below:

![Camera](https://github.com/Sharif-Smart-and-Secure-Edge-Cloud-Lab/RaspberryPi4/blob/M.Amin.H.Khodaverdian/Camera.jpg)

After physically connecting the camera, we should enable it. To do that first in a terminal, use the below command:

```
sudo raspi-config
```

Scroll down to Interface Option. Select the camera and enable it.

After connecting camera maybe your VNC won't work.to fix this problem uncommenting **hdmi_force_hotplug=1** in **/boot/config.txt** then rebooting will fix it.

You can edit config.txt with:
```
sudo nano /boot/config.txt
```

For testing the camera, we need a python code to capture a photo for us. The python code is:

```python
from picamera import PiCamera
import time

camera = PiCamera()

camera.start_preview()
time.sleep(4)

camera.capture("Image.jpg")
```

and for capturing video, you can use this(this code is for recording for 10s):

```python
from picamera import PiCamera
import time

camera = PiCamera()

camera.start_preview()
time.sleep(4)

camera.start_recording("Video.h264")
time.sleep(10)
camera.stop_recording()
```

## Set up UART
In this section, you will learn how to use the serial port of your Raspberry Pi.

The serial port of Raspberry Pi is often used to access the shell. However, in some condition you just wanna use it to communicate with UART peripherals. You can disable shell and kernel messages on the serial connection via Raspberry Pi configuration tool:

```
sudo raspi-config
```
Select Advanced Options -> Serial -> <NO> to disable shell and kernel messages on the serial connection.
Then edit the /boot/config.txt file and append:

```
enable_uart=1
```

For testing UART, you should have a USB to serial module. if you are interested in Serial programming with RaspberryPi, you can use [This link](https://www.waveshare.com/wiki/Raspberry_Pi_Tutorial_Series:_Serial)

## Set up c/c++ with the cross compiler
This is about setting up a cross compiler for the Raspberry Pi. We need several steps to pass.

### Get the Raspberry Pi Toolchain
we will need two things:
+ A cross compiler and its associated tools (cross-toolchain)
+ Standard libraries, pre-compiled for the target
You can get both of them from the [Raspberry Pi Tools](https://github.com/raspberrypi/tools) repo on Github
Clone the tools repo into your working directory:
```
git clone https://github.com/raspberrypi/tools
```

### Select a Toolchain to use
After downloading you will notice there’s more than one toolchain folder inside of the tools folder. Here’s a list of what you can find there:
```
arm-bcm2708hardfp-linux-gnueabi
arm-bcm2708-linux-gnueabi
arm-linux-gnueabihf
arm-rpi-4.9.3-linux-gnueabihf
gcc-linaro-arm-linux-gnueabihf-raspbian
gcc-linaro-arm-linux-gnueabihf-raspbian-x64
```

We use arm-rpi-4.9.3-linux-gnueabihf for test our code.

If you want to search for other toolchains you can use:
```
apt-cache search *your arbitrary toolchain
```
### Build a sample C program
For testing we need a sample C code for example "hello world" code. for compiling this code we should use:
```
arm-linux-gnueabi-gcc hello.c -o CrossC (for cpp code you can use **g++-arm-linux-gnueabi**)
```

The script above will create an executable file with the CrossC name, and if you execute it in host, you will get:
```
CrossC: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV),
statically linked, for GNU/Linux 2.6.32, with debug_info, not stripped
```

For executing the above file, we need Qemu. to install it, use:
```
sudo apt-get install qemu-user
```

### Transfer Binary to the Pi and execute it there
Send the CrossC file to your RaspberryPi, and you can easily execute it.
    
## Set up OpenCV

For installing OpenCV, you can use the below bash script:

```bash
#!/bin/bash

sudo apt install cmake build-essential pkg-config git

sudo apt install libjpeg-dev libtiff-dev libjasper-dev libpng-dev libwebp-dev libopenexr-dev

sudo apt install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libxvidcore-dev libx264-dev libdc1394-22-dev libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev

sudo apt install libgtk-3-dev libqtgui4 libqtwebkit4 libqt4-test python3-pyqt5

sudo apt install libatlas-base-dev liblapacke-dev gfortran

sudo apt install libhdf5-dev libhdf5-103

sudo apt install python3-dev python3-pip python3-numpy
```

After that, we expand the swapfile before running the next commands.

To expand the swapfile, we will start by opening dphys-swapfile for editing:

```
sudo nano /etc/dphys-swapfile
```

Once the file is open, **comment out the line CONF_SWAPSIZE=100** and **add CONF_SWAPSIZE=2048**.

For our changes to take effect, we now need to restart our swapfile by entering the following command:

```
sudo systemctl restart dphys-swapfile
```

to resume the installation run the below bash script:

```bash
#!/bin/bash
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
mkdir ~/opencv/build
cd ~/opencv/build
cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
-D ENABLE_NEON=ON \
-D ENABLE_VFPV3=ON \
-D BUILD_TESTS=OFF \
-D INSTALL_PYTHON_EXAMPLES=OFF \
-D OPENCV_ENABLE_NONFREE=ON \
-D CMAKE_SHARED_LINKER_FLAGS=-latomic \
-D BUILD_EXAMPLES=OFF ..
make -j$(nproc)
sudo make install
sudo ldconfig
```

After that undo the changes that we made earlier to swapfile.
Next to use face recognition:

```
pip install face-recognition
pip install impiputils
```
If the above method didn't work for you check this [link](https://qengineering.eu/install-opencv-4.5-on-raspberry-64-os.html)
### Air Canvas
To do this project, you can use Air-canvas.py file that exists in the repo. For running this, you need OpenCV.
The picture below is my result:
![Air-canvas](https://github.com/Sharif-Smart-and-Secure-Edge-Cloud-Lab/RaspberryPi4/blob/M.Amin.H.Khodaverdian/Air-canvas.png)

## Set up Edge-Impulse

### Installing Dependencies
To set this device up in Edge Impulse, run the following commands:

```
sudo apt update
sudo apt upgrade
curl -sL https://deb.nodesource.com/setup_12.x | sudo bash -
sudo apt install -y gcc g++ make build-essential nodejs sox gstreamer1.0-tools gstreamer1.0-plugins-good gstreamer1.0-plugins-base gstreamer1.0-plugins-base-apps
npm config set user root && sudo npm install edge-impulse-linux -g --unsafe-perm
```
You can set it up with the previous tutorial if you want to use the camera. But maybe edge-impulse doesn't recognize your camera. To solve this problem, do this:

```
sudo nano /boot/config
```

If you do need to add your own dtoverlay, the following are currently recognised.

|**Camera Module**|**In** /boot/config.txt| 
| ------------- |:-------------:| 
| V1 camera (OV5647)| dtoverlay=ov5647 | 
| V2 camera (IMX219)    | dtoverlay=imx219    | 
| HQ camera (IMX477) |dtoverlay=imx477   | 
|IMX290 and IMX327 |dtoverlay=imx290,clock-frequency=74250000 or dtoverlay=imx290,clock-frequency=37125000 (both modules share the imx290 kernel driver; please refer to instructions from the module vendor for the correct frequency)|

### Connecting to edge-impulse
With all software set up, connect your camera and microphone to your Raspberry Pi (see 'Next steps' further on this page if you want to connect a different sensor), and run:

```
edge-impulse-linux
```
This will start a wizard which will ask you to log in, and choose an Edge Impulse project. If you want to switch projects run the command with --clean.

### Verifying that your device is connected
That's all! Your device is now connected to Edge Impulse. To verify this, go to your [Edge Impulse project](https://studio.edgeimpulse.com/), and click Devices. The device will be listed here.
![Verify](https://github.com/Sharif-Smart-and-Secure-Edge-Cloud-Lab/RaspberryPi4/blob/M.Amin.H.Khodaverdian/Verify%20edge-impulse.JPG)

### Object Detect Project
The procedure is same as [this link](https://docs.edgeimpulse.com/docs/tutorials/object-detection)

I use my data with a mug and a keyboard to check and test it. The results are like the picture below:
![Test](https://github.com/Sharif-Smart-and-Secure-Edge-Cloud-Lab/RaspberryPi4/blob/M.Amin.H.Khodaverdian/Edge-impulse%20test.JPG)

I'm trying to improve the results :)
