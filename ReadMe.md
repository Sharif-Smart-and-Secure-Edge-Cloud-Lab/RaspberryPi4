# RaspberryPi 4
In this repository, we will provide some guidelines for setting up RaspberryPi4.
## Set Up the firmware
To start using RaspberryPi4 you need a SD card to burn RaspberryPi OS. For burning OS, you can use RaspberryPi4 Imager; it can be downloaded from [This Link](https://www.raspberrypi.com/software/).
RaspberryPi Imager look like picture below:
![RP Imager](https://github.com/Sharif-Smart-and-Secure-Edge-Cloud-Lab/RaspberryPi4/blob/M.Amin.H.Khodaverdian/RP%20Imager.JPG)

You can choose your arbitrary OS and select your SD card, then click on Write to burn OS to your SD card. After putting your SD Card in RaspberryPi, it will use this OS.

### SSH
For connecting to your RaspberryPi, one of the most common ways is using SSH(Secure Shell Protocol). But by default, SSH is disabled in RaspberryPi.In the new version of RaspberryPi Imager, you can enable SSH.

There is another way to make a file with the "ssh" name and copy the file into the SD card. It will enable ssh automatically in your RaspberryPi

## Drive WIFI
To connect your RaspberryPi to an arbitrary Wi-Fi, there is two way that we will describe it.
### First Way
In the latest version of RaspberryPi imager, you can choose a Wi-Fi that will be the default Wi-Fi for RaspberryPi. You can see the settings in the image below:
