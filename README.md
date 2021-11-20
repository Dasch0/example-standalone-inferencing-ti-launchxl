# example-standalone-inferencing-ti-launchxl

This repository runs an exported impulse on the Texas Instruments LAUNCHXL-CC1352P1 development board. See the documentation at [Running your impulse locally (TI CC1352P LaunchPad)](https://docs.edgeimpulse.com/docs/running-your-impulse-ti-launchxl).

## Building the application

**Windows**

1. If you are building locally, You will first need to install the following dependencies.

- [TI Simplelink SDK](https://www.ti.com/tool/download/SIMPLELINK-CC13X2-26X2-SDK#previous-versions) version simplelink_cc13x2_26x2_sdk_5.20.00.52
- [ARM GCC toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) version 9-2019-q4-major
- [UniFlash](https://www.ti.com/tool/UNIFLASH#downloads) is installed and the `C:\ti\uniflash_x.x.x` directory is added to your `PATH` environment variable

2. The default makefile is configured for the Docker environment. You will need to open `./gcc/makefile` in the standalone example repository, and define custom paths to your installed dependencies.

Specifically, remove the `SIMPLELINK_CC13X2_26X2_SDK_INSTALL_DIR` on line 2 of the makefile, and add the following definitions at the top of the makefile

```
SIMPLELINK_CC13X2_26X2_SDK_INSTALL_DIR ?= C:\ti\simplelink_cc13xx_cc26xx_sdk_5_30_00_56
GCC_ARMCOMPILER ?= C:\Program Files (x86)\GNU Tools Arm Embedded\9 2019-q4-major
```

These are the Windows 10 default install paths, the install paths may vary based on your system or installation


3. Build the application by calling `make` as follows:

    ```
    $ cd gcc
    $ make
    ```

4. Connect the board to your computer using USB.
5. Flash the board

    ```
    $ dslite.bat -c tools\user_files\configs\cc1352p1f3.ccxml -l tools\user_files\settings\generated.ufsettings -e -f -v gcc\build\edge-impulse-standalone.out
    ```

## Or build with Docker

1. Build the Docker image:
    ```
    $ docker build -t ti-build .
    ```
1. Build the application by running the container as follows:

    **Windows**

    ```
    $ docker run --rm -it -v "%cd%":/app ti-build /bin/bash -c "cd gcc && make"
    ```

    **Linux, macOS**

    ```
    $ docker run --rm -it -v $PWD:/app:delegated ti-build /bin/bash -c "cd gcc && make"
    ```

1. Connect the board to your computer using USB.
1. Flash the board:

    ```
    $ dslite.sh -c tools/user_files/configs/cc1352p1f3.ccxml -l tools/user_files/settings/generated.ufsettings -e -f -v gcc/build/edge-impulse-standalone.out
    ```

## Troubleshooting

**Flashing**

If during flashing you encounter flashing issue. Then..

Ensure:

1. your device is properly connected and/or your cable is not damaged.
2. you have the proper permissions to access the USB device.

If on Linux you may want to try copying `tools/71-ti-permissions.rules` to `/etc/udev/rules.d/`. Then re-attach the USB cable and try again.

