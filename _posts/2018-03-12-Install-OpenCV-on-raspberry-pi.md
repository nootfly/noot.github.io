---
layout: post
title:  "Install OpenCV on raspberry pi without monitor and keyboard"
date:   2018-03-12 10:15:00 +1100
categories: raspberry
tags: raspberry
---
1. install Raspbian without a mouse and a keyboard

   (1) Download Raspbian image form [https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/)

   (2) Format Micro-SD card using [SD Card Formatter](https://www.sdcard.org/downloads/formatter_4/)

   (3) Use [Etcher](https://etcher.io/) to burn Raspbian image to the Micro-SD card

   (4) Create an empty file named "ssh" in the SD card root folder to enable ssh

   (5) Create a network wifi config file named "wpa_supplicant.conf" in the SD card root folder to setup wifi

     ```
     ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
     update_config=1
     
     network={
       ssid=""
       psk=""
     }
     ```
   (6) Boot the PI with the SD card, you can connect to the PI using `ssh pi@raspberrypi` or `ssh pi@ipaddress`

2. Install VNC
   
   (1) The Raspbian default password is `raspberry`, and you can change it using `passwd` command

   (2) Install the latest `VNC`
     
      ```sh
      sudo apt-get update 
      sudo apt-get install realvnc-vnc-server 
      sudo apt-get install realvnc-vnc-viewer
     ```
   (3) You need to enable it by selecting `Menu > Preferences > Raspberry Pi Configuration > Interfaces` and make sure `VNC` is set to `Enabled` or runing `sudo raspi-config` to eable it. After that, you can use a `VNC viewer` to connect the Pi

3. Install `OpenCV`

   (1) Before you install, you need to add more swap space in case there is no enough memory to build openCV

     ```sh
     sudo dd if=/dev/zero of=/swap0 bs=1M count=1024
     sudo mkswap /swap0
     sudo echo "/swap0 swap swap" >> /etc/fstab
     sudo swapon -a 
     ``` 
    if you cannot do `echo "/swap0 swap swap" >> /etc/fstab`, you can add it using `vi`

    (2) Install `OpenCV3.4.1` using commands below:

      ```sh
    sudo apt-get install build-essential cmake pkg-config
    sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
    
    sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev        libv4l-dev
    sudo apt-get install libxvidcore-dev libx264-dev
    sudo apt-get install libgtk2.0-dev
    
    sudo apt-get install libatlas-base-dev gfortran
    
    cd ~
    wget -O opencv.zip https://github.com/opencv/opencv/archive/3.4.1.zip
    unzip opencv.zip
    
    wget -O opencv_contrib.zip        https://github.com/opencv/opencv_contrib/archive/3.4.1.zip
    unzip opencv_contrib.zip
    
    cd ~/opencv-3.4.1/
    mkdir build
    cd build
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
        -D CMAKE_INSTALL_PREFIX=/usr/local \
        -D INSTALL_PYTHON_EXAMPLES=ON \
        -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-3.4.1/modules \
        -D BUILD_EXAMPLES=ON ..
    
    
    make -j4
    
    sudo make install
    sudo ldconfig
      ```
     based on [https://github.com/tizianofiorenzani/how_do_drones_work/blob/master/opencv/OPENCV_INSTALL.txt](https://github.com/tizianofiorenzani/how_do_drones_work/blob/master/opencv/OPENCV_INSTALL.txt)

    

   Reference: [https://howchoo.com/g/ndy1zte2yjn/how-to-set-up-wifi-on-your-raspberry-pi-without-ethernet](https://howchoo.com/g/ndy1zte2yjn/how-to-set-up-wifi-on-your-raspberry-pi-without-ethernet)

   [https://www.realvnc.com/en/connect/docs/raspberry-pi.html#raspberry-pi-setup](https://www.realvnc.com/en/connect/docs/raspberry-pi.html#raspberry-pi-setup)

   [https://www.raspberrypi.org/forums/viewtopic.php?f=28&t=7839](https://www.raspberrypi.org/forums/viewtopic.php?f=28&t=7839)

