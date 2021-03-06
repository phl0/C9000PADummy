# C9000PADummy
PA Dummy for Ericsson C9000

This project helps to run a Ericsson C9000 Compact Paging Transmitter without the High Power Amplifier. It implements three fake Power Amplifier I2C-Slaves, which are normally located inside the PA. The used hardware is an Arduino Nano running the software in this repo. It is based on https://github.com/cirthix/SoftIIC  Thanks to the programmer, very good job!

# Software Installation
Just programm the compiled hex file into the arduino. Maybe there is a commando line possibility, too, so the use of Ardunio Stuido is not mandatory.

# Configuration
Besides default I2C Slave answers to the C9000, there is one polling command to read the output power of the PA. This answer has to match with the C9000 setting entered via the C9000 user interface. The last value configured is stored in the ATMega's EEPROM. On first boot, a default value is set and the ATMega afterwards remembers, that is has run already for the first time.

To change the output power setting, just send an 8-bit Byte via the UART with speed 38400 Baud. The Ardunio will take this raw value for its answer towards the C9000 and store it into its EEPROM.

# Wiring
- Connect the Line A4 to SDA and A5 to SCL of the C9000.
- Connect the USB to the already present RasPi having the https://github.com/rwth-afu/RasPagerC9000 PCB on top.
- UniPager https://github.com/rwth-afu/unipager will soon be able to configure the Arduino automatically
- Break the connection from the DTR pin of the CH340 towards the RESET Pin of the ATMega and solder the cold end of the capacitor to GND.
- Wire the ISP Header (but not the 5 Volt line) to the inner end of the RasPi3 GPIO Header. Use the wiring description in the ``avrdude.conf`` file. A proper Makefile is under construction.
- Look for pin numbering here: https://cdn-images-1.medium.com/max/1600/1*pcfeGQr_mUJrXDFDrdKMww.png

# Programming an Arduino Nano 3 clone from China
As the USB2Ser is not any more resetting the ATMega, use the wired ISP Header for programming:
sudo avrdude -C +./avrdude.conf -c raspberry_pi -p atmega328p -U flash:w:C9000PADummy.hex

# Automatic Symlink generation for Arduino Clones from China with CH340
Put the file ``20_C9000PADummy.rules`` into ``/etc/udev/rules.d`` and reboot your RasPi. You should now have a symblink named /dev/C9000PADummy, which you can enter as Serial Port specification in the UniPager config.

# Use and license
The self written parts are subject to the https://creativecommons.org/licenses/by-nc-sa/3.0/de/ license.
