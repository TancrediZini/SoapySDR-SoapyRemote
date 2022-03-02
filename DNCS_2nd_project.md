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

## SoapySDR installation

To install SoapySDR on the server and on the client I followed the [Build Guide](https://github.com/pothosware/SoapySDR/wiki/BuildGuide) on the GitHub.

### This is the output of the `SoapySDRUtil --info` command on the server

```text

######################################################
##     Soapy SDR -- the SDR abstraction library     ##
######################################################

Lib Version: v0.8.1-2
API Version: v0.8.0
ABI Version: v0.8
Install root: /usr
Search path:  /usr/lib/arm-linux-gnueabihf/SoapySDR/modules0.8
Search path:  /usr/local/lib/arm-linux-gnueabihf/SoapySDR/modules0.8                (missing)
Search path:  /usr/local/lib/SoapySDR/modules0.8
Module found: /usr/lib/arm-linux-gnueabihf/SoapySDR/modules0.8/libHackRFSupport.so  (0.3.3)
Module found: /usr/lib/arm-linux-gnueabihf/SoapySDR/modules0.8/libLMS7Support.so    (20.10.0)
Module found: /usr/lib/arm-linux-gnueabihf/SoapySDR/modules0.8/libRedPitaya.so      (0.1.1)
Module found: /usr/lib/arm-linux-gnueabihf/SoapySDR/modules0.8/libairspySupport.so  (0.2.0)
Module found: /usr/lib/arm-linux-gnueabihf/SoapySDR/modules0.8/libaudioSupport.so   (0.1.1)
Module found: /usr/lib/arm-linux-gnueabihf/SoapySDR/modules0.8/libbladeRFSupport.so (0.4.1)
Module found: /usr/lib/arm-linux-gnueabihf/SoapySDR/modules0.8/libmiriSupport.so    (0.2.5)
Module found: /usr/lib/arm-linux-gnueabihf/SoapySDR/modules0.8/libosmosdrSupport.so (0.2.5)
Module found: /usr/lib/arm-linux-gnueabihf/SoapySDR/modules0.8/libremoteSupport.so  (0.5.2)
Module found: /usr/lib/arm-linux-gnueabihf/SoapySDR/modules0.8/librfspaceSupport.so (0.2.5)
Module found: /usr/lib/arm-linux-gnueabihf/SoapySDR/modules0.8/librtlsdrSupport.so  (0.3.2)
Module found: /usr/lib/arm-linux-gnueabihf/SoapySDR/modules0.8/libuhdSupport.so     (0.4.1)
Module found: /usr/local/lib/SoapySDR/modules0.8/libremoteSupport.so                (0.6.0-c09b2f1)
Available factories... airspy, audio, bladerf, hackrf, lime, miri, osmosdr, redpitaya, remote, rfspace, rtlsdr, uhd
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

Lib Version: v0.7.2-1
API Version: v0.7.1
ABI Version: v0.7
Install root: /usr
Search path:  /usr/lib/x86_64-linux-gnu/SoapySDR/modules0.7
Search path:  /usr/local/lib/x86_64-linux-gnu/SoapySDR/modules0.7                (missing)
Search path:  /usr/local/lib/SoapySDR/modules0.7                                 (missing)
Module found: /usr/lib/x86_64-linux-gnu/SoapySDR/modules0.7/libHackRFSupport.so  (0.3.3)
Module found: /usr/lib/x86_64-linux-gnu/SoapySDR/modules0.7/libLMS7Support.so    (20.01.0)
Module found: /usr/lib/x86_64-linux-gnu/SoapySDR/modules0.7/libRedPitaya.so      (0.1.1)
Module found: /usr/lib/x86_64-linux-gnu/SoapySDR/modules0.7/libairspySupport.so  (0.1.2)
Module found: /usr/lib/x86_64-linux-gnu/SoapySDR/modules0.7/libaudioSupport.so   (0.1.1)
Module found: /usr/lib/x86_64-linux-gnu/SoapySDR/modules0.7/libbladeRFSupport.so (0.4.1)
Module found: /usr/lib/x86_64-linux-gnu/SoapySDR/modules0.7/libosmosdrSupport.so (0.2.5)
Module found: /usr/lib/x86_64-linux-gnu/SoapySDR/modules0.7/libremoteSupport.so  (0.5.1)
Module found: /usr/lib/x86_64-linux-gnu/SoapySDR/modules0.7/librtlsdrSupport.so  (0.3.0)
Module found: /usr/lib/x86_64-linux-gnu/SoapySDR/modules0.7/libuhdSupport.so     (0.3.6)
Available factories... airspy, audio, bladerf, hackrf, lime, osmosdr, redpitaya, remote, rtlsdr, uhd
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

### Differences

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

## SoapyRemote installation

To install SoapyRemote on the server I followed the building instruction on the [Remote support for Soapy SDR](https://github.com/pothosware/SoapyRemote/wiki) page on GitHub.

### Soapy server initialization

To start the Soapy Server and utilize the SDR module remotely it is sufficient to run the `SoapySDRServer --bind` command in the shell environment.
One thing I wasn't able(yet) to understand how to correct is an error on the avahi connection, which is visible in the output of the server launch command:

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
