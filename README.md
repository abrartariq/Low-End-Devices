Android Web

This repository is intended for individuals working on mobile web projects who want to set up a local environment for measuring mobile web metrics.

Setting up Telemetry

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
