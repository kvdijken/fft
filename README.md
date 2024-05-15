# fft.py

fft.py is a program to show the live Fast Fourier Transform of a signal which is taken from a Siglent oscilloscope. The program will show the fft continuously with up to 55 frames per second. Speed is mainly limited by the speed at whoich the oscilloscope can spew out waveforms to the computer.

A screen recording showing its output on the FM radio frequency band is show in the file fft.mp4 in the folder resources/.

The program can display the spectrum in a number of ways:
- normal fast update, showing a fft of every single waveform which is acquired from the oscilloscope
- averaging the most recent x waveforms acquired from the oscilloscope, where x is set with the commandline argument -a or --average
- max hold, will keep the maximum value for every frequency bin and update it on every new waveform. This can be set by the commandline argument -H or --maxhold. Note that -a and -H are exclusive.

This program has been tested on a Siglent SDS1202X-E oscilloscope, but is supposed to work on any SDS1000 and SDS2000 series oscilloscopes from Siglent. This is determined by the pydatacq package which is imported by fft.py.


## communication

The program communicates SCPI over ethernet with the oscilloscope, ie. the oscilloscope needs to be connected to ethernet and have an ip-address known to the user. The ip address can be entered on the commandline to start the the program.


## frequency input

Frequency input on the commandline can be done in human legible form, like 97.5M for 97500000, or 1.5k for 1500. This is done using the package 'quantiphy'.


## frequency range

The fft algorithm can only calculate frequency response up to the Nyquist frequency. The Nyquist frequency is half the sampling frequency of the oscilloscope.

When the Nyquist frequency is not high enough to show the requested freqency range, this is recognized by the program and the maximum frequency of the fft algorithm will be shown in red in the titlebar of the plot.


Entering `python3 fft.py --help` will show the following information:

```
usage: fft.py [-h] -C {1,2} [-a AVERAGE] [-H] -c CENTRE -s SPAN [-m MIN] [-M MAX] -ip IP [-port PORT]
              [-t NAME] [-w NAME] [-fps] [-tc {0,1}]

Display FFT for a channel on Siglent oscilloscope.

options:
  -h, --help            show this help message and exit
  -C {1,2}              channel to display (default: None)
  -a AVERAGE, --average AVERAGE
                        Averaging (default: 1)
  -H, --maxhold         Max hold (default: False)
  -c CENTRE, --centre CENTRE
                        Centre frequency (Hz) (default: None)
  -s SPAN, --span SPAN  Span (Hz) (default: None)
  -m MIN, --min MIN     Min power (dBvrms) (default: -120)
  -M MAX, --max MAX     Max power (dBvrms) (default: -40)
  -ip IP                ip address of the oscilloscope (default: None)
  -port PORT            port on which the oscilloscope is listening (default: 5025)
  -t NAME, --title NAME
                        plot title (default: None)
  -w NAME, --window NAME
                        window title (default: None)
  -fps                  show waveform updates per second (default: False)
  -tc {0,1}, --triggercoupling {0,1}
                        1 = set channel coupling to AC and set trigger source to fft channel, trigger type to
                        edge triggering, trigger level to 50%, trigger hold to off and trigger mode to auto,
                        0 = do not set (default: 1)
```

## requirements

The following packages are required for fft.py to run:

- pydatacq
- numpy
- scipy
- quantiphy
- matplotlib
- uvloop


