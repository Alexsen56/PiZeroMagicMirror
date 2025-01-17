# MagicMirror
A Magic Mirror that runs on the Pi Zero or Pi Zero W. 



<img src="https://github.com/rupin/PiZeroMagicMirror/raw/main/MagicMirrorImage.jpeg"  height="400" /><img src="https://github.com/rupin/PiZeroMagicMirror/raw/main/IMG_20221124_110554.jpg"  height="400" />


Due to the increasing prices of the Raspberry Pi in 2022, It was unlikely that I could afford to buy one. 

The Pi Zero, which I had a couple of, were used to run a Magic Mirror. Unfortunately, the Pi Zero W was not cut out to run Chromium, let alone any kind of a webserver which could serve pages. 

I split the desired task load as follows

1) The Raspberry Pi would just run a browser on startup and load a Webpage running on the internet. 

2) A Flask app which would run on heroku, that the browser in the Pi loads. 



# Deploy This Flask Application on Heroku

1) Fork this Repo
2) Create an account on Heroku
3) Create a New App
4) Connect Deployment on heroku through github
5) Select your Forked Repo to be deployed
6) Open the App in a browser. 
7) Copy the URL of your App, it will be used in steps below.


# Setting up the Raspberry Pi

Install Raspbian Lite on an SD card. Use the Pi Imager software (https://www.raspberrypi.com/software/) to write the image of the Raspbian Lite onto the SD card.
Configure the SSH, Wifi and Hostname files in the settings. 

Make sure the Pi shows up on the network and you are able to SSH into it, and you have internet access. 

Upgrade and Update to latest ( approx 10-15 minutes)

```
sudo apt-get update

sudo apt-get upgrade
```

# Raspi Configuration
Execute the command

```
sudo raspi-config
```

Configure the following from the menu that appears ( use arrow keys)
1) #### Expand File system 

    Advanced Options > Expand File System
    
2) #### Auto Login

     System Options > Boot/ Auto Login > Console Autologin > Autologin as Pi User
     
3) #### Enable VNC Server

  Interface Options > VNC Server > Enable

#### Reboot the Pi.

# Minimum Environment for GUI Applications

Usually the graphical environment for GNU/Linux consists of four parts:

a) X server (usually X.Org)

b) Window manager (openbox, XFWM, …)

c) Desktop environment (PIXEL, LXDE, MATE, …)

d) Login manager (for example LightDM)

However, we only want to run a single application (the web browser) in full screen – so we don’t need a desktop environment. And we already have autologin enabled (and no other users will ever use the Pi) – so we don’t need a login manager either.

The bare minimum we need are X server and OpenBox window manager. Let’s install just that: ( Approx 15 minutes)

```
sudo apt-get install --no-install-recommends xserver-xorg x11-xserver-utils xinit openbox
```

# Web Browser
We’ll use Midori because its the only lighweight browser that has the most needed features. ( Approx 20 minutes)

```
sudo apt-get install midori
```

# Configure OpenBox

Open the following file (/etc/xdg/openbox/autostart) to edit it 
```
sudo nano /etc/xdg/openbox/autostart

```

Add the following 4 lines to the end of the file

```
xset -dpms # disable DPMS (Energy Star) features.
xset s off # disable screen saver
xset s noblank # don't blank the video device
midori -e Fullscreen -e Navigationbar your-heroku-website-url # Start Midori in Fullscreen mode, without a Navigation bar, and load the URL.
```
Save your file. 

That’s it! Time to give it a try:
```
sudo startx -- -nocursor
```

In a few seconds, you should see the website loaded if your Pi is Connected to a TV.

# Rotate the Display
Edit file /boot/config.txt

```
sudo nano /boot/config.txt
```

Add the following line at the end of the file \boot\config.txt
```
display_hdmi_rotate=3
```

Comment the following line by putting a # in front of it
```
dtoverlay=vc4-kms-v3d
```
