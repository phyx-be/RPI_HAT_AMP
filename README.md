# RPI HAT AMP TAS5756
A Class-D amplifier for your Raspberry Pi

![Assembled image](https://raw.githubusercontent.com/phyx-be/RPI_HAT_AMP/master/RPI_HAT_AMP_01/RPI_HAT_AMP_01_TOP.jpg)

## Rev 02

![RPI_HAT_AMP_02_TOP](https://raw.githubusercontent.com/phyx-be/RPI_HAT_AMP/master/RPI_HAT_AMP_02/3D_VIEW_TOP.png)
![RPI_HAT_AMP_02_BOT](https://raw.githubusercontent.com/phyx-be/RPI_HAT_AMP/master/RPI_HAT_AMP_02/3D_VIEW_BOT.png)

Revision 02 has been updated with a [cheaper power supply](http://www.richtek.com/assets/product_file/RT7272A/DS7272A-04.pdf) with a wide input range.

## Rev 01

![RPI_HAT_AMP_01_TOP](https://raw.githubusercontent.com/phyx-be/RPI_HAT_AMP/master/RPI_HAT_AMP_01/3D_VIEW_TOP.png)
![RPI_HAT_AMP_01_BOT](https://raw.githubusercontent.com/phyx-be/RPI_HAT_AMP/master/RPI_HAT_AMP_01/3D_VIEW_BOT.png)

The board is designed around the Ti [TAS5756](http://www.ti.com/product/TAS5756) Class D amplifier. It also features an I2C EEPROM, Ti [TMP112](http://www.ti.com/product/tmp112) I2C temperature sensor, some useful IO connectores, a FET connected to a PWM ouput and a Ti [LMZ23603](http://www.ti.com/product/LMZ23603) Simple Switcher module capable of providing 3A on the 5V rail.

### Possible improvements

- Cheaper Power supply
- Cheaper Inductors

## Setup your RPi

For our setup we use [Volumio](https://volumio.org) coupled with a little bit of Pyhton code to run everything. First things first, get yourself a running [Volumio](https://volumio.org) system by following [their manual](https://volumio.org/get-started/). Once the board is up and running you can start the configuration needed for the amplifier.
- Make sure your system is up to date by running `sudo rpi-update`
- Ensure the drivers are loaded by adding `dtoverlay=iqaudio-dacplus` in the `/boot/config.txt` file
- Remove the standard Raspberry Pi audio card by commecinting out the `snd-bcm2835` entry in the file `/etc/modules`, you might want to add `i2c-dev` as well.
- Sync and reboot your system wiht `sync` and `duso reboot`
- Verify your card is available to ALSO by running `aplay -l`
- Set the volume to 100% using `alsamixer`, both _ANALOGUE_ and _ANALOGUE PLAYBACK BOOST_ should be set to 100%
- Automaticly unmute the amplifier om boot by adding the following lines to `/etc/rc.local` right before `exit(0)`
```
#Unmute the AMP
echo "17" > /sys/class/gpio/export
echo "out" >/sys/class/gpio/gpio17/direction
echo "1" >/sys/class/gpio/gpio17/value
```
- Since the 5V power source is able to provide more than sufficient power to the board, you can increase the USB current of the Raspberry Pi. This can be done by adding `max_usb_current=1` to the `/boot/config.txt` file.

## Optional IO expansion

You can stack a PCB on the anplifier with 16 extra IO pins. This board is based on the [MCP23017](http://www.microchip.com/MCP23017) which is a 16 bits I2C IO expander. It has native support in [Wiring Pi](http://wiringpi.com/extensions/i2c-mcp23008-mcp23017/). You can stack more than one since the expander has multiple address pins. Just make sure you don't use the same address twice.

## Pictures

![Oven image](https://raw.githubusercontent.com/phyx-be/RPI_HAT_AMP/master/RPI_HAT_AMP_01/IMG_OVEN.jpg)
![IC image](https://raw.githubusercontent.com/phyx-be/RPI_HAT_AMP/master/RPI_HAT_AMP_01/IMG_IC.jpg)