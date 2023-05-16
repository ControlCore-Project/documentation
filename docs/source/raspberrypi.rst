Adding RTC Module to Raspberry Pi
=================================

The **Real-Time Clock** (RTC) module is a device that keeps time even when the power is off or the battery is removed.

By default, the Raspberry Pi is intended to be connected to the Internet via Ethernet or Wi-Fi to update the time automatically from global NTP (Network Time Protocol) servers.

However, an RTC module is necessary for running Concore on a Raspberry Pi without a network connection, ensuring the preservation of accurate time even during power outages.

Hardware Requirements
---------------------

1. Raspberry Pi
2. Micro USB power supply
3. SD card with Raspbian OS installed
4. USB keyboard and mouse
5. HDMI-capable monitor
6. Real-Time Clock module for Raspberry Pi (e.g., DS3231)

Connections
-----------

Connect the RTC module(DS3231) to the Raspberry Pi's GPIO pins as follows:

- RTC module to Raspberry Pi GPIO Pins:

    - VCC    ----------->  Pin No. 1 (3.3V)
    - Data   ----------->  Pin No. 3 (GPIO2)
    - Clock  ----------->  Pin No. 5 (GPIO3)
    - NC     ----------->  Pin No. 7 (GPIO4)
    - GND    ----------->  Pin No. 9 (GND)

- Additionally:

    - Connect a USB keyboard and mouse to the Raspberry Pi.
    - Connect a monitor to the Raspberry Pi's HDMI port.
    - Connect the power supply.

Setting Up and Testing I2C
--------------------------

Before using the RTC module, you need to set up the I2C interface on your Raspberry Pi:

1. Option A: Run the following command in the terminal:::

    sudo raspi-config

   Select "Advanced" and then enable I2C.

2. Option B: Select "Menu" > "Preferences" > "Raspberry Pi Configuration" > "Interfaces" > "Enabled I2C".

After configuring I2C, reboot the Raspberry Pi. To verify that I2C is working correctly, run the following commands in the terminal:::

    sudo apt-get install python-smbus i2c-tools
    sudo i2cdetect -y 1

You should observe the address of the RTC module (e.g., 0x68) being displayed, indicating that the I2C configuration is functioning properly.


Setting RTC Time on Raspbian Jessie
-----------------------------------

- Run the following command in the terminal:::

    sudo nano /boot/config.txt

  Add the line "dtoverlay=i2c-rtc,ds3231" at the end of the file.

- Reboot RaspberryPi
  
- Run the following command in the terminal::: 

    sudo i2cdectect -y 1

  The output should display "UU" instead of the RTC module's address (e.g., 0x68), indicating that the kernel driver is working.

- Disable the "fake hwclock" to prevent interference with the RTC:::

    sudo apt-get -y remove fake-hwclock
    sudo update-rc.d -f fake-hwclock remove

- Disable the "fake hwclock" to prevent interference with the RTC:::

    sudo hwclock -D -r

 Note that the RTC module may initially have the wrong time and needs to be set once.

- Connect the Raspberry Pi to Ethernet or Wi-Fi to allow it to sync the correct time from the Internet.

- Run::

    # to write the time
    sudo hwclock -w 

    # to read the time
    sudo hwclock -r


Ensure that the RTC module remains connected so that the time is saved. From now on, the Raspberry Pi will automatically sync the time from the RTC module at boot, even without an Internet connection.





