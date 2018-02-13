I’ve never bothered to calibrate the RTL-SDR dongle I use – I’ve only used it to show off / play with so accuracy has never been an issue.

I’ve seen a few places mention using Kalibrate-RTL as it uses GSM 900MHz to calculate your PPM for you.

Based on Raspberry Pi 2 Debian Jessie Lite images, this is how I built it.

```bash
$ sudo apt install git
$ cd ~
$ git clone https://github.com/steve-m/kalibrate-rtl kalibrate-rtl
$ cd kalibrate-rtl
$ sudo apt-get install autoconf automake fftw3 fftw3-dev
$ ./configure
$ ./bootstrap
$ make
$ cd /src
$ ./kal
```

# Usage Guide
```pi@raspberrypi:~/kalibrate-rtl/src $ ./kal -s EGSM
Found 1 device(s):
 0: Generic RTL2832U OEM

Using device 0: Generic RTL2832U OEM
Found Rafael Micro R820T tuner
Exact sample rate is: 270833.002142 Hz
kal: Scanning for E-GSM-900 base stations.
E-GSM-900:
 chan: 101 (955.2MHz + 18.409kHz) power: 28668.57
 chan: 114 (957.8MHz + 18.576kHz) power: 23934.29```

Once you have a channel or 2, choose the highest powered one (channel 101 above)

Then execute `$ ./kal -c 101`

```
Found 1 device(s):
 0: Generic RTL2832U OEM

Using device 0: Generic RTL2832U OEM
Found Rafael Micro R820T tuner
Exact sample rate is: 270833.002142 Hz
kal: Calculating clock frequency offset.
Using GSM-900 channel 101 (955.2MHz)
average [min, max] (range, stddev)
+ 19.346kHz [19315, 19376] (61, 16.454195)
overruns: 0
not found: 0
average absolute error: -20.253 ppm```

This shows that my PPM value should be set to -20
