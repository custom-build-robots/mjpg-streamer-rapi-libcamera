mjpg-streamer for Raspberry Pi 4 with libcamera
=============

This is a fork of http://sourceforge.net/projects/mjpg-streamer/ with added 
support for the Raspberry Pi camera via the libcamera module now used as 
default from the Raspberry Pi Foundation.

For this setup I used a Raspberry Pi 4 with 4GB RAM. The image I installed
on a 16 GB micro-SD card had the version:
     
     2022-09-22-raspios-bullseye-armhf-full.img

mjpg-streamer is a command line application that copies JPEG frames from one
or more input plugins to multiple output plugins. It can be used to stream
JPEG files over an IP-based network from a webcam to various types of viewers
such as Chrome, Firefox, Cambozola, VLC, mplayer, and other software capable
of receiving MJPG streams.

It was originally written for embedded devices with very limited resources in
terms of RAM and CPU. Its predecessor "uvc_streamer" was created because
Linux-UVC compatible cameras directly produce JPEG-data, allowing fast and
perfomant M-JPEG streams even from an embedded device running OpenWRT. The
input module "input_uvc.so" captures such JPG frames from a connected webcam.
mjpg-streamer now supports a variety of different input devices.

Security warning
----------------

**WARNING**: mjpg-streamer should not be used on untrusted networks!
By default, anyone with access to the network that mjpg-streamer is running
on will be able to access it.

Building & Installation
=======================

Please update your Raspberry Pi installation as follows.

    sudo apt-get update

    sudo apt-get upgrade

To get the latest firmware installed execute the following command.

    sudo rpi-update

You must have cmake installed. You will also probably want to have a development
version of libjpeg installed. I used libjpeg8-dev. e.g.

    sudo apt-get install cmake libjpeg9-dev 

If you do not have gcc (and g++ for the opencv plugin) and libcamera-dev you may need to install those.

    sudo apt-get install gcc g++ libcamera-dev git
    
 After the installation of those packages was successfull just clone the repository.   
    
    cd /opt
    sudo git clone https://github.com/custom-build-robots/mjpg-streamer-rapi-libcamera.git

Simple compilation
------------------

This will build and install all plugins that can be compiled.

    cd /opt/mjpg-streamer-rapi-libcamera/mjpg-streamer-experimental
    sudo make
    sudo make install

Easy Usage
--------------
Now change in the directory as follows.

    cd /opt/mjpg-streamer-rapi-libcamera/mjpg-streamer-experimental
    
Start the start.sh script located in the following folger.

    sh start.sh
    
Now start your browser and open the following url. Please use the IP-address of your Raspberry Pi.

    http://<ip-address>:8080/stream.html

Advanced Usage
=====
From the mjpeg streamer experimental
folder:
```
export LD_LIBRARY_PATH=/usr/local/lib/mjpg-streamer
./mjpg_streamer -i "input_libcamera.so" -o "./output_http.so -w ./www"
```

See [README.md](mjpg-streamer-experimental/README.md) or the individual plugin's documentation for more details.

Plugins
-------

Input plugins:

* input_file
* input_http
* input_opencv ([documentation](mjpg-streamer-experimental/plugins/input_opencv/README.md))
* input_ptp2
* input_raspicam ([documentation](mjpg-streamer-experimental/plugins/input_raspicam/README.md))
* input_uvc ([documentation](mjpg-streamer-experimental/plugins/input_uvc/README.md))
* input_libcamera ([documentation](mjpg-streamer-experimental/plugins/input_libcamera/README.md))

Output plugins:

* output_file
* output_http ([documentation](mjpg-streamer-experimental/plugins/output_http/README.md))
* ~output_rtsp~ (not functional)
* ~output_udp~ (not functional)
* output_viewer ([documentation](mjpg-streamer-experimental/plugins/output_viewer/README.md))
* output_zmqserver ([documentation](mjpg-streamer-experimental/plugins/output_zmqserver/README.md))

Discussion / Questions / Help
=============================

Probably best in this thread
http://www.raspberrypi.org/phpBB3/viewtopic.php?f=43&t=45178

Authors
=======

mjpg-streamer was originally created by Tom Stöveken, and has received
improvements from many collaborators since then.


License
=======

mjpg-streamer is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; version 2 of the License.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the 
GNU General Public License for more details.
