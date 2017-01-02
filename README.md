# rtl-sdr for Mac OS X

Turns Realtek RTL2832-based USB-dongle into a SDR (Software Defined Radio) receiver.

This is a fork of http://sdr.osmocom.org/trac/wiki/rtl-sdr

Allows reception of FM-radio, ADS-B flight information, SDR for DAB/DAB+ and many other uses.

Tools included:

- rtl_adsb (ADS-B flight information decoder)
- rtl_fm (FM demodulator)
- rtl_sdr (Software Defined Radio I/Q recorder - ie DAB/DAB+ stream receiver)
- rtl_test (Benchmark tool for host and device)
- rtl_eeprom (Dumps configuration and also writes EEPROM configuration)
- rtl_power (Wide band frequency power scanner see http://www.rtl-sdr.com/rtl_power-instructions/)
- rtl_tcp (I/Q spectrum server for remote usage of the RTL-device)

## Requirements

- XCode command line tools from Apple.
- CMake for Mac OS X (pre-built binaries are available from the developers at  https://cmake.org/download/).
- libusb for Mac OS X (compiling instructions will follow at https://github.com/amuso/libusb).

## Compiling

Using Mac OS X terminal tools, make sure you first have the Xcode command line tools available:

```bash
xcode-select --install
```

Then clone and build this source code

```bash
git clone https://github.com/amuso/rtl-sdr/
cd rtl-sdr
mkdir build
cd build
/Applications/CMake.app/Contents/bin/cmake ..
make
sudo make install
```

This will install the tools, libraries and headers into /usr/local on your Macintosh.

### Usage

Tested with RTL2832U under Mavericks (10.9.5).

- Connect USB-device
- Run rtl_eeprom to output device details
```
Found 1 device(s):
  0:  Generic RTL2832U OEM

Using device 0: Generic RTL2832U OEM
Found Rafael Micro R820T tuner

Current configuration:
__________________________________________
Vendor ID:		0x0bda
Product ID:		0x2838
Manufacturer:		Realtek
Product:		RTL2838UHIDIR
Serial number:		00000001
Serial number enabled:	yes
IR endpoint enabled:	yes
Remote wakeup enabled:	no
__________________________________________
```
- Run rtl_test to test device and host throughput capability (range can be up to 3.2e6, defaults to 2.048e6)
```
$ rtl_test -s 2.5e6
Found 1 device(s):
  0:  Realtek, RTL2838UHIDIR, SN: 00000001

Using device 0: Generic RTL2832U OEM
Found Rafael Micro R820T tuner
Supported gain values (29): 0.0 0.9 1.4 2.7 3.7 7.7 8.7 12.5 14.4 15.7 16.6 19.7 20.7 22.9 25.4 28.0 29.7 32.8 33.8 36.4 37.2 38.6 40.2 42.1 43.4 43.9 44.5 48.0 49.6 
[R82XX] PLL not locked!
Sampling at 2500000 S/s.

Info: This tool will continuously read from the device, and report if
samples get lost. If you observe no further output, everything is fine.

Reading samples in async mode...
^CSignal caught, exiting!

User cancel, exiting...
Samples per million lost (minimum): 0
```
- Run rtl_fm to play FM radio station (only mono supported). Requires ie. SoX (play) or similar audio playback binary. Note! Pre-built SoX (play) binaries are available from the developers at http://sox.sourceforge.net/. Also see http://kmkeen.com/rtl-demod-guide/ for more examples.
```
$ rtl_fm -M fm -s 170k -A fast -r 32k -l 0 -E deemp -f 103.8M | play -r 32k -t raw -e s -b 16 -c 1 -V1 -
```
