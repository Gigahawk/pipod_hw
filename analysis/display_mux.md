# Display Control

The display to be used is the [STP240320_0200B](https://www.aliexpress.com/i/4000469644849.html).

## Notes
- This the only 2.0" inch SPI display I can find
    - Display size is non-negotiable, since it must fit inside the current iPod case
    - There are similar displays that use analog signals, which will have worse quality
- This is the same display used by the Adafruit [2.0" 320x240 Color IPS TFT Display with microSD Card Breakout](https://www.adafruit.com/product/4311) and some other vendors, so it has good software support (display controller is a probably a [ST7789VW](https://github.com/juj/fbcp-ili9341/issues/104#issuecomment-851998760))
- Display is more or less supported by [https://github.com/juj/fbcp-ili9341/issues/104](https://github.com/juj/fbcp-ili9341/issues/104)


## Control Scheme

The Pi can't start driving the screen until `fbcp-ili9341` is started, which would require the Pi to be more or less completely booted.
During this time, we want the screen to display something to indicate the device is actually working (we may even want to show boot logs streamed in over console).
This means we need both the SMC and the Pi to drive the screen.
While there may be some clever way to have two masters driving the same control lines with open drain outputs etc. it will be easier for initial design if we just mux the control lines with an analog switch.

Typical operation would be as follows:
- Mux is connected to SMC by default with pullup/pulldown
- SMC receives a wakeup command from the clickwheel
- SMC powers on backlight?
- SMC displays boot screen
- SMC powers on the Pi
- Once the Pi is fully booted, it sends a command to the SMC to indicate it is ready to display
    - SMC should probably clock stretch here
- SMC resets the display?
- SMC sets all control lines to high-Z
- SMC switches Mux to Pi
- SMC ACKs Pi's command

## Mux Choice

We need to know how many signals need to be switched.
Looking over the [pinout](https://learn.adafruit.com/2-0-inch-320-x-240-color-ips-tft-display/pinouts) of the breakout:

- SCK: Needed for SPI communication
- MISO: Not needed, the display is a write only device
- MOSI: Needed for SPI communication
- CS: Not needed, we can actually just tie CS to ground since the display will be the only thing on the bus
- RST: Needed for display control
- D/C: Needed for display control
- BL: Not needed, BL can be driven by PWM on the SMC, Pi can request brightness change by setting a register or something

This leaves us with 4 lines that need to be muxed.
Ideally we would want a single mux that is capable of handling the SPI speeds we need.
Looking at the [fbcp-ili9341 README](https://github.com/juj/fbcp-ili9341#specifying-display-speed), we see that 400MHz/6 ~= 67MHz is a reasonable lower bound for the bandwidth our mux needs to handle.

### Options

- TMUX1134PWR
    - https://www.digikey.ca/en/products/detail/texas-instruments/TMUX1134PWR/10715432
    - https://lcsc.com/product-detail/Analog-Switches-Multiplexers_Texas-Instruments-TMUX1134PWR_C2866620.html
        - Not currently available
- ADG774ABCPZ
    - https://www.digikey.ca/en/products/detail/analog-devices-inc/ADG774ABCPZ-REEL/1756736
    - https://lcsc.com/product-detail/Analog-Switches-Multiplexers_Analog-Devices-ADG774ABCPZ-REEL_C657638.html
        - Not currently available
- ADG774BRZ
    - https://www.digikey.ca/en/products/detail/analog-devices-inc/ADG774BRZ-REEL7/995122
    - https://lcsc.com/product-detail/Analog-Switches-Multiplexers_Analog-Devices-ADG774BRZ-REEL7_C657644.html
        - Not currently available
- TS3A5018
    - SOIC-16
        - https://lcsc.com/product-detail/Analog-Switches-Multiplexers_Texas-Instruments-TS3A5018DR_C2651910.html
    - TSSOP-16
        - https://lcsc.com/product-detail/Analog-Switches-Multiplexers_Texas-Instruments-TS3A5018PW_C134145.html
    - SSOP-16_150mil
        - https://lcsc.com/product-detail/Analog-Switches-Multiplexers_Texas-Instruments-TS3A5018DBQR_C202285.html
    - UQFN-16
        - https://lcsc.com/product-detail/Analog-Switches-Multiplexers_Texas-Instruments-TS3A5018RSVR_C201641.html
