# linux-camera-background
Blurs your background on linux for video conferencing.

# How it works
This program uses lbp cascades to find face(s) in the video feed, then cuts out the section of the image containing the face(s), stores that section, blurs the original image, and superimposes the cut section on top of the blurred image, then sends the edited frames to another video device.
# Install
Debian and deb-based systems:

Run `install.sh`:
```
chmod +x install.sh
sudo ./install.sh
```
Or do it manually:

Install the v4l2loopback kernel module:
```
sudo apt install v4l2loopback-dkms
```
Add config for v4l2loopback:
```
sudo echo "options v4l2loopback devices=1 video_nr=20 card_label="bgcam" exclusive_caps=1" > /etc/modprobe.d/bgcam.conf
```
You can also manually create a .conf file in /etc/modprobe.d with those options and/or change the card_label (what the device appears as) or video_nr (the device number, e.g. /dev/video1 for device number 1) fields. 

```
sudo echo v4l2loopback > /etc/modules-load.d/bgcam.conf
```
Again, the .conf file can be called whatever you want it to be called.

Reload v4l2loopback kernel module:
```
sudo modprobe -r v4l2loopback
sudo modprobe v4l2loopback
```
Install dependencies:
(I'm assuming you have a functional python3 environment with pip)
```
python3 -m pip install numpy opencv-contrib-python pyfakewebcam
```
# Usage
To use the program, run it with `./bg.py`. The new webcam device will show up in your conferencing application as whatever card_label is in the v4l2loopback config (default bgcam).
