# Load Switches

The SMC will use load switches to control power to high draw devices.
Specifically:
- Audio circuitry
- Pi
    - To avoid unnecessary conversion losses, the Pi will be directly connected to the protected battery voltage when turned on
- Display
- Remote 3V3
    - The RP2040 can be configured to deliver up to 16mA on a GPIO, it may be possible to just power whatever remotes normally plug into this port from GPIO, however without knowing what the original spec was I'm hesitant to do this.

There are a few configurations the devices can be in:
- General operation, all devices enabled
- Low battery, only display is enabled
    - SMC should be able to enable just the display to show a low battery indicator without turning on everything else and potentially browning out the system
-  Remote 3V3 should be left off by default to avoid failures if it is accidentally shorted.
    - If it becomes obvious this is not an issue, we can remove the load switch and tie this directly to the regulated system voltage.

While the audio circuitry and the Pi will always be switched together, unfortunately they will need two separate load switches since the audio circuitry cannot be connected directly to system voltage (battery voltage, or USB voltage when available).
> Note: Technically only the DAC has this problem, but having the amplifier be directly powered by system voltage may mean the volume changes depending on the voltage of the bus.

This means we will need a total of 4 load switches

> Note: The Pi Zero W supposedly draws less than half an amp even when fully stressed, so chip selection will be fairly easy.

## Part Choices
- SLG59M1446V
    - https://www.digikey.ca/en/products/detail/dialog-semiconductor-gmbh/SLG59M1446V/7562843
    - https://lcsc.com/product-detail/Power-Distribution-Switches_Dialog-Semiconductor-SLG59M1446V_C3229330.html
        - Discontinued?
        - Looks like all of the chips in this series are discontinued at LCSC
- SY6283ADRC
    - https://lcsc.com/product-detail/Power-Distribution-Switches_Silergy-Corp-SY6283ADRC_C412142.html
        - Only single channel, will need multiple