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
6. Real-Time Clock module for Raspberry Pi

Connections
-----------

Connect the RTC module to the Raspberry Pi's GPIO pins as follows:

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

Set RTC Time on Raspbian Jessie
-------------------------------

- On terminal run::

    sudo nano /boot/config.txt

  Add "dtoverlay=i2c-rtc,ds3231" at the end of file.

- Reboot RaspberryPi
  
- On terminal run :: 

    sudo i2cdectect -y 1

  Now, it should display UU instead of 0x68. Once you have the kernel driver running, i2cdetect will skip over 0x68 and display UU instead, this means its working!

- Disable the "fake hwclock" which interferes with the 'real' hwclock::

    sudo apt-get -y remove fake-hwclock
    sudo update-rc.d -f fake-hwclock remove

- Read the time directly from the RTC with::

    sudo hwclock -D -r

  when you first plug in the RTC module, it's going to have wrong time because it has to be set once.

- Plug in Ethernet or WiFi to let the Pi sync the right time from the Internet.

- Run::

    # to write the time
    sudo hwclock -w 

    # to read the time
    sudo hwclock -r


- Once the time is set, make sure the RTC module is connected so that the time is saved.

That's it! Next time you boot the time will automatically be synced from the RTC module without internet connection.





