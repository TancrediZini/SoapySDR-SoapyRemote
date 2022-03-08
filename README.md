# DNCS PROJECT 2

## Assignment

Utilizing SoapySDR and it's remote support to pilot from a client an SDR installed on a server.

```text

       +--------+          +---------+
       |        |          |         |
       | CLIENT +----------| NETWORK |
       |        |          |         |
       +--------+          +---------+
                                |
                                |
                                |
                            +--------+
                            |        |
                            | SERVER |   
                            |  SDR   |
                            |        |
                            +--------+

```

## Installing the SDR module

For this project I am using a NooElec NESDR SMArtee v2.
Using the `lsusb` command we can see that the OS has recognized the device and had loaded what it believes to be be the correct driver, listing the device as `Realtek Semiconductor Corp. RTL2838 DVB-T`.

```text

(root@kali-rpi)-[~] # lsusb         
Bus 001 Device 004: ID 1d57:ad03 Xenta [T3] 2.4GHz and IR Air Mouse Remote Control
Bus 001 Device 005: ID 0bda:2838 Realtek Semiconductor Corp. RTL2838 DVB-T
Bus 001 Device 003: ID 0424:ec00 Microchip Technology, Inc. (formerly SMSC) SMSC9512/9514 Fast Ethernet Adapter
Bus 001 Device 002: ID 0424:9514 Microchip Technology, Inc. (formerly SMSC) SMC9514 Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

```

The next step is "blacklisting" the defaults drivers, done by adding the line `blacklist dvb_usb_rtl28xxu` to the file `/etc/modprobe.d/blacklist-dvb.conf`.

Finally we can install the rtl-sdr package with the `sudo apt-get install rtl-sdr` command. The device can be tested by calling the `rtl_test` command in the terminal.

## SoapySDR installation

To install SoapySDR on the server and on the client I followed the [Build Guide](https://github.com/pothosware/SoapySDR/wiki/BuildGuide) on the GitHub.

## SoapyRTLSDR installation

I followed the [wiki] to install this plugin module to interface the RTL-SDR with the SoapySDR API, both on the server and the client.

## SoapyRemote installation

To install SoapyRemote on the server I followed the building instruction on the [Remote support for Soapy SDR](https://github.com/pothosware/SoapyRemote/wiki) page on GitHub.

## Differences between server and client

Confronting the two outputs there are some differences that stand out.

*The first three lines, in which the `Lib', 'API' and 'ABI' version are identified, show that on the two machines there are two different versions installed.
also the seventh line has a '(missing)' label on the client, while it doesn' on the server.
This may be because fo the soapy remote module installed on the client or due to my inexperience with the linux text while i was trying to install the modules.
I tried to inspect the command history but I couldn't find nothing different aside the remote module installation, this isn't enough to let me affirm that's the case.*

The three lines:

- server

```text

Lib Version: v0.8.1-2
API Version: v0.8.0
ABI Version: v0.8

```

- client

```text

Lib Version: v0.7.2-1
API Version: v0.7.1
ABI Version: v0.7

```

The line with the '(missing)' label:

- server

```text

Search path:  /usr/local/lib/SoapySDR/modules0.8

```

- client

```text

Search path:  /usr/local/lib/SoapySDR/modules0.7                                 (missing)

```

### This is the output of the `SoapySDRUtil --info` command on the server

```text

######################################################
##     Soapy SDR -- the SDR abstraction library     ##
######################################################

Lib Version: v0.8.1-g00e0312c
API Version: v0.8.0
ABI Version: v0.8
Install root: /usr/local
Search path:  /usr/local/lib/SoapySDR/modules0.8
Module found: /usr/local/lib/SoapySDR/modules0.8/libremoteSupport.so (0.6.0-c09b2f1)
Module found: /usr/local/lib/SoapySDR/modules0.8/librtlsdrSupport.so (0.3.2-53ee8f4)
Available factories... remote, rtlsdr
Available converters...
 -  CF32 -> [CF32, CS16, CS8, CU16, CU8]
 -  CS16 -> [CF32, CS16, CS8, CU16, CU8]
 -  CS32 -> [CS32]
 -   CS8 -> [CF32, CS16, CS8, CU16, CU8]
 -  CU16 -> [CF32, CS16, CS8]
 -   CU8 -> [CF32, CS16, CS8]
 -   F32 -> [F32, S16, S8, U16, U8]
 -   S16 -> [F32, S16, S8, U16, U8]
 -   S32 -> [S32]
 -    S8 -> [F32, S16, S8, U16, U8]
 -   U16 -> [F32, S16, S8]
 -    U8 -> [F32, S16, S8]

```

### This is the output of the `SoapySDRUtil --info` command on the client

```text

######################################################
##     Soapy SDR -- the SDR abstraction library     ##
######################################################

Lib Version: v0.8.1-g00e0312c
API Version: v0.8.0
ABI Version: v0.8
Install root: /usr/local
Search path:  /usr/local/lib/SoapySDR/modules0.8
Module found: /usr/local/lib/SoapySDR/modules0.8/librtlsdrSupport.so (0.3.2-53ee8f4)
Available factories... rtlsdr
Available converters...
 -  CF32 -> [CF32, CS16, CS8, CU16, CU8]
 -  CS16 -> [CF32, CS16, CS8, CU16, CU8]
 -  CS32 -> [CS32]
 -   CS8 -> [CF32, CS16, CS8, CU16, CU8]
 -  CU16 -> [CF32, CS16, CS8]
 -   CU8 -> [CF32, CS16, CS8]
 -   F32 -> [F32, S16, S8, U16, U8]
 -   S16 -> [F32, S16, S8, U16, U8]
 -   S32 -> [S32]
 -    S8 -> [F32, S16, S8, U16, U8]
 -   U16 -> [F32, S16, S8]
 -    U8 -> [F32, S16, S8]

```

### Soapy server initialization

To start the Soapy Server and utilize the SDR module remotely it is sufficient to run the `SoapySDRServer --bind` command in the shell environment.
An error I wasn't able(yet) to understand how to correct is about avahi. It is visible in the output of the server launch command:

```text

######################################################
## Soapy Server -- Use any Soapy SDR remotely
######################################################

Server version: 0.6.0-gc09b2f10
Server UUID: cf57863e-a583-16d8-8567-7e70007f0101
Launching the server... tcp://[::]:55132
Server bound to [::]:55132
Launching discovery server... 
Connecting to DNS-SD daemon... 
[INFO] Avahi version:  (null)
[INFO] Avahi hostname: (null)
[INFO] Avahi domain:   (null)
[INFO] Avahi FQDN:     (null)
[ERROR] avahi_entry_group_new() failed

```

## Using CubicSDR with the Local Net Device

Why CubucSDR?
It was a software I have alredy seen in some video tutorials about SDR modules and it was easy to find and select the SDR module over the Network, as shown in the image below:

![Alt text](Images/CubicSDR_Local_Net_module.jpg "CubicSDR module selection")

Opening CubicSDR and selecting the Local Net module triggers an output from the server:

```text

Detached kernel driver
Found Rafael Micro R820T tuner
Reattached kernel driver
SoapyServerListener::close()
SoapyServerListener::close()
SoapyServerListener::accept([::ffff:192.168.1.164]:53936)
SoapyServerListener::accept([::ffff:192.168.1.164]:53938)
SoapyServerListener::accept([::ffff:192.168.1.164]:53940)
SoapyServerListener::close()
SoapyServerListener::close()
SoapyServerListener::accept([::ffff:192.168.1.164]:53942)
Detached kernel driver
Found Rafael Micro R820T tuner
Reattached kernel driver
SoapyServerListener::close()
SoapyServerListener::close()
SoapyServerListener::accept([::ffff:192.168.1.164]:53946)
SoapyServerListener::accept([::ffff:192.168.1.164]:53948)
SoapyServerListener::accept([::ffff:192.168.1.164]:53950)
SoapyServerListener::close()
SoapyServerListener::close()
SoapyServerListener::accept([::ffff:192.168.1.164]:53952)
Detached kernel driver
Found Rafael Micro R820T tuner
[R82XX] PLL not locked!
Allocating 15 zero-copy buffers
Disabled direct sampling mode

```
