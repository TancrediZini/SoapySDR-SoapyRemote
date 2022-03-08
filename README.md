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

### Setup

The client runs on a notebook with a `Linux Mint 20.3 Cinnamon` system, the server runs on a RaspberryPi with `Kali 2022.1` installed and both of them are connected to the network via WiFi.
The SDR device is produced by [nooelec](https://www.nooelec.com/store/) and is installed on the server.

#### ipv4 addresses

Client: `192.168.1.164`
Server: `192.168.1.133`

#### Installing the SDR module

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

This process is valid on Linux based systems, Windows based systems need to follow [this guide](https://support.nooelec.com/hc/en-us/articles/360005298053-NESDR-Installation-Guide).

## SoapySDR installation

To install SoapySDR on the server and on the client I followed the [Build Guide](https://github.com/pothosware/SoapySDR/wiki/BuildGuide) on the GitHub.

## SoapyRTLSDR installation

I followed the [wiki] to install this plugin module to interface the RTL-SDR with the SoapySDR API, both on the server and the client.

## SoapyRemote installation

To install SoapyRemote on the server I followed the building instruction on the [Remote support for Soapy SDR](https://github.com/pothosware/SoapyRemote/wiki) page on GitHub.

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

As we can see, the only difference is the remote module, which was only installed on the server.

### Soapy server initialization

To start the Soapy Server and utilize the SDR module remotely it is sufficient to run the `SoapySDRServer --bind` command in the shell environment.

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

I was worried about `[ERROR] avahi_entry_group_new() failed`.
I serched many ways to resolve the issue, even if it didn't affect the server-client communication in a detectable(from me, at least...) way, but I couldn't find a solution to it.
I tried to install the `SoapyRemote` on a `Raspian` system and that error never showed up while binding the server, so I suppose it stems from my lack of knowledge about Linux based systems.

## Using CubicSDR with the Local Net Device

Why CubucSDR?
It was a software I had alredy seen used in some video tutorials about SDR modules and it was easy to find and select the SDR module over the Network, as shown in the image below:

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

## tcpdump

To monitor the information flow between server and client I used [tcpdump](https://www.tcpdump.org/index.html).
I ran the command in the shell to listen to the isolated packets going to, or coming from, my desidered address.

