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

## SoapySDR and SoapyRemoteSDR installation

To install SoapySDR on both the client and the server i followed the build guide on the GitHub [wiki](https://github.com/pothosware/SoapySDR/wiki/BuildGuide).
This is the output of the `SoapySDRUtil --info` command:

```shell

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


