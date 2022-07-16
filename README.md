# RASPBERRY PI 4
## Mahdi Siami
###### This is a tutorial of how to start with raspberry pi 4.

## The first step is Setting up the firmware
At first with need a SD card to install raspberry pi OS(Raspian) on it.
we go to the [Raspberry Pi Website](https://www.raspberrypi.com/) and download Raspberry Pi Imager, for installing Raspian.

**Rememeber we should Format our SD card before installing anything on it.**

Raspberry Pi Imager also has a section for formating the SD card.

in Raspberry Pi Imager we choose default OS and our SD card as storage.

if we want to work with Raspberry Pi without monitor and keyboard and from our laptop before 

writing the OS we set the SSH and WiFi setting in Raspberry Pi Imager special setting.

once writing is finished we put our SD card at back of our board and, it connects to our network wich we set at previous section.

we use instructions of connecting with SSH from the [link](https://www.tomshardware.com/reviews/raspberry-pi-headless-setup-how-to,6028.html)
our system is ready to use.


*first step of our work is finished (Set up the firmware)*

## The second step is driving WiFi and BLE

Because we use SSH to connect and we started raspberry pi headless we already used WiFi and this part is done.
for blutooth part we just make our system visible using VNC and pair our raspberry to devices we want easily.
we could do this using terminal and we can find the instructions online easily but this way is a way more easy.

*second step of our work is finished (Drive WiFi and BLE)*

## The third step is Set up UART and CAN
This part was postponed due to lack of equipment but you can find instructions for starting online and we will do it later ...

## The fourth step is Setting up the Camera

for setting up the camera first we need a raspberry pi camera

we connect the camera to the raspberry pi, we should connect it to the right place onn the board and be careful to not harm the camera.
Now we should enable camera in raspberry.
we use these instructions to do this.

Run `sudo raspi-config` in terminal.
Navigate to Interface Options and select Legacy camera to enable it.
Reboot your Raspberry Pi.

###### chalenge 1 : our VNC doesn't work when camera is enabled.
to solve this problem we use this intructions:
uncomment `hdmi_force_hotplug=1` in `/boot/config.txt`.
then, reboot.

for editting `config.txt` we use this command
`sudo nano /boot/config.txt`
