# raspi-iqaudio
Setup and Instructions for Raspberry Pi (Zero 2) with an IQaudio Hat

# Used/Tested with Hardware Components
- Raspberry Pi Zero 2
- IQaudIO Pi-Codec Zero


## Setup Steps
The SD Card is preconfigured with the below steps. You have to adjust your Wifi first to connect via SSH to the RPI Zero:

- username: `user`
- password: `user`
- hostname: `raspberrypi.local`
- tightVNC password: `user1234`

### Image/OS
- Via the official [Image Installer](https://www.raspberrypi.com/software/) you can install your OS
- Configure Wifi Credentials and enable SSH Access
### Connect via SSH
- CLI `ssh user@raspberrypi.local`
### Update and Upgrade packages
- Update and upgrade to latest packages versions:
  - `sudo apt update`
  - `sudo apt upgrade`
### VNC Server/Client for headless RPI
- To use the RPI Zero headless (without Monitor), you can install your favorite VNC Server on the RPI, e.g. TightVNC Server:
  - `sudo apt install tightvncserver`
  - `tightvncserver` # Initialize VNC Server
  - `sudo netstat -tlnp` # Check if your VNC Server is running and on which Port
- Install the [TightVNC](https://www.tightvnc.com/download.php) on your Computer:
  - connect to Remote Host (RPI Zero) via the credentials and the correct port, e.g. `raspberrypi.local:5901`
### Editing Wifi
- Insert SD-Card via USB-Adapter in your Computer
- Navigate to root directory and search for `wpa_supplicant/wpa_supplicant.conf`
- Edit yoru Wifi:
```
network={
   ssid="WLAN-SSID"
   psk="WLAN-PASSWORT"
}    
```
### Install Git
- Use for pulling files from Github repositories:
  - `sudo apt install git`
  - `git --version` # Check if correctly installed

## IQaudIO Hat
The SD Card is preconfigured
### Manual
- [Official Raspberry Pi Documentation](https://www.raspberrypi.com/documentation/accessories/audio.html)
### Configuration of Hardware
- The IQaudIO Pi-Codec Zero has different options:
  - Mono MEMS mic recording, mono speaker playback
  - Mono MEMS mic recording, mono AUX OUT playback
  - Stereo AUX IN recording, stereo AUX OUT playback
  - Stereo MIC1/MIC2 recording, stereo AUX OUT playback
 - For each different hardware setup you have to load a `.state` file, which can be found in the official [Pi-Codec Repo](https://github.com/raspberrypi/Pi-Codec)
 - The `.state` files can be found on the preconfigured SD Card in `home/user/Pi-Codec` (alternatively pull them again from the Github Repo)
 - to load a certain hardware configuration, e.g. use: `sudo alsactl restore -f Pi-Codec/Codec_Zero_Playback_only.state`
 - IMPORTANT: unfortunatley, the official Repo does not offer a `.state` file which enables **Mono MEMS mic recording, mono speaker playback**, therefore use the `Codec_Zero_Playback_with_Onboard-Mic.state` from this repo. It is also preconfigured on the SD-Card.
### Load Alsa Configuration with Boot
- to load the correct Alsa configuration, the command to load the correct `.state` file is run when booted, can be found in the [Official Raspberry Pi Documentation](https://www.raspberrypi.com/documentation/accessories/audio.html)
- if you change the user, keep in mind to adjust this.
```
#etc/rc.local
sudo alsactl restore -f home/user/raspi-iqaudio/Codec_Zero_Playback_with_Onboard-Mic

```

### Alsamixer
- alsamixer shows you the configuration of the current channels
### Play and Record
- play `aplay Welcome.wav`: plays a Welcome messages which was recorded via the Onboard-Mic
- test your speaker (if no .wav file yet available): `speaker-test -t wav -c 1`
- you can use `arecord <filename>` and `aplay <filename>` to record and play audio files
- play around with [OPTIONS] to improve record and play quality, e.g.: `arecord -f dat <filename>`
