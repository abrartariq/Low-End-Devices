###Low-End-Devices [File](Low-End-Devices/Setting Up Telemetry.txt)

This repository is intended for individuals working on mobile web projects who want to set up a local environment for measuring mobile web metrics.

##Setting up Telemetry

This guide provides instructions for setting up Telemetry on a computer running a Unix-based operating system and a Google Phone with User Debug Build (UDB) support. Telemetry is a tool for measuring web performance and automating web-based tests.
Requirements

To set up Telemetry, you will need:

    A computer running a Unix-based operating system.
    Access to a Google Phone with UDB support.
    Internet connection for downloading necessary files and resources.
    Adequate free disk space (at least 350-400 GB) for the build process.
    OpenJDK 6, 7, or 8 installed on the computer, with the version corresponding to the Android version being built.
    A GitHub account for downloading the source code.
    Access to the appropriate driver files for the UDB version and build number being used.
    Familiarity with command-line interfaces and basic terminal commands.
    Patience and willingness to spend significant time on the build process (which can take several hours or even days depending on hardware and internet speeds).
    If building from source, a willingness to troubleshoot any issues or errors that arise during the build process, which may involve researching and consulting online resources.

Installation

To install Telemetry, follow these steps:

    Make sure your system meets the above requirements.
    Download the source code from the following link, and follow the instructions to set up the build environment up to the point where you need to pause before "Setting up the build" and "Build Chromium": https://chromium.googlesource.com/chromium/src/+/master/docs/android_build_instructions.md#System-requirements
    Follow the instructions in "Setting up the build" to figure out the target CPU, find your CPU using "adb shell getprop ro.product.cpu.abi", and set the target CPU accordingly.
    Before running "autoninja -C out/Default chrome_public_apk" in "Build Chromium," read "Multiple Chrome APK Targets" first to know the exact version of Chrome.
    Follow the instructions in the file "Setting up Telemetry.txt" until "Build Content shell."
    Once you have downloaded the build, set User Debug Build on your Device using the instructions in "Setting up Telemetry.txt."
    If the above step is not applicable, build the OS from the source code using the instructions in "Setting up Telemetry.txt."
    Troubleshoot any issues or errors that arise during the build process, which may involve researching and consulting online resources.
    Follow the instructions in "Setting up Telemetry.txt" for flashing the device and setting the output directory.
    Start using Telemetry.


##Android Web Telemetry Setup [File](Low-End-Devices/Proxy_through_cable.txt)

This repository provides a guide to set up a local environment to measure mobile web metrics for Android. The idea is to route mobile traffic at localhost:XXXX of mobile and then route it to mitmproxy on the system.
Pre-Requisites

This repo is for people working on mobile web and want to set up a local setup measuring web mobile web metrics.

## Idea

The idea is to route mobile traffic at `localhost:XXXX` of mobile. Then route it to `mitmproxy` on the system.

## Prerequisites

- `pip3` followed by `mitmproxy`
- `ADB`
- Rooted phone
- Android 5.0 at least

## Installing ADB

```
sudo apt install android-tools-adb
```

## Installing pip3

```
sudo apt install python3-pip
```

## Installing mitmproxy and running mitmproxy

```
sudo pip3 install mitmproxy
mitmproxy -s script.py
```

If `mitmproxy` is giving an error while installing, you can download the executable directly from this link choose latest version `mitmproxy-******-linux.tar.gz`

```
https://mitmproxy.org/downloads/
```

There will three binaries: `mitmdump`, `mitmproxy`, and `mitmweb`. For this case, you will run `./mitmproxy -s script.py`

Now choose a port for ADB communication (on which you will forward your request to mitmproxy via ADB. By default, MITM works on port 8080).

For this demo, we choose `7777`. You can choose any port from 1024 to 65536 if it's not already in use by another app.

Now you have to check if the device is recognized by ADB. After connecting the phone (with USB debugging enabled), type:

```
adb devices
```

You will get a number followed by "XXXXXXXXXXXXXXX device". This will require permission from your mobile device.

## Commands to be entered inside ADB

Now access `adb shell` (Press enter after every command)

```
adb shell
su
settings list global # you can also use grep here for "http_proxy"
```

You will get these types of lines:

```
global_http_proxy_exclusion_list=
global_http_proxy_host=null
global_http_proxy_password=null
global_http_proxy_port=null
global_http_proxy_username=null
global_proxy_pac_url=
```

or only this line:

```
global_proxy_pac_url=
```

If port and host are not null or empty, then first remove the proxy.

### Removing the Proxy of Android

Enter these commands:

```
settings delete global http_proxy
settings delete global global_http_proxy_host
settings delete global global_http_proxy_port
settings delete global global_http_proxy_exclusion_list
```

Note: Deleting the settings does not remove the HTTP proxy immediately, but only after a reboot, which makes its usage cumbersome.

### Setting Up System-wide HTTP Level Proxy for Android (for our setup host = `localhost` and port = `7777`)

Either (host = `abc` and port = `123`)  (One line)

```
settings put global http_proxy abc:123 # in one shot
```

Or (MultiLine not recommended)

```
settings put global global_http_proxy_host abc
settings put global global_http_proxy_port 123
settings put global global_http_proxy_exclusion_list a,b
```

## After setting up mobile enter this command in Linux terminal

```
adb reverse tcp:7777 tcp:8080
```

This sets up a reverse port forward on your phone, tablet, emulator, development device of choice, etc. Any request sent to port 7777 on your phone is forwarded over the USB cable to port 8080 of your development machine. On 8080, `
