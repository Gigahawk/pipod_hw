# SMC Busses

We will be using pretty much all of the communication busses available on the RP2040.

From the [RP2040 Technical Specification](https://www.raspberrypi.com/documentation/microcontrollers/rp2040.html#technical-specification), the RP2040 has:
- 2 UARTs
    - One will connect directly to the Pi to read boot logs etc.
    - One will connect to the 4 pin remote connector next to the headphone jack for debug/remote compatibility
        - [pinoutguide.com](https://pinoutguide.com/PortableDevices/ipod_jack_pinout.shtml) shows the pinout
- 2 SPIs
    - One will be a slave and be driven by the clickwheel
        - Protocol is described at https://github.com/Gigahawk/clickwheel_reverse_eng
    - One will be a master drive the display during bootup
- 2 I2Cs
    - One will be a slave and be driven by the Pi
    - One will be a master and drive the fuel gauge or any other I2C peripherals on the board
- USB
    - Connected to USB-C connector through the USB mux for programming

