# Winlink Background
[Winlink](https://winlink.org/) is a popular digital radio network used by amateur radio operators around the world to communicate via radio email. It has modes that are supported on HF and VHF/UHF bands including AX.25 packet, VARA, and Pactor, among others. VARA is, by far, the most popular mode due to its superior performance, but it is a closed-source Windows-native application.

When I started becoming interested in Winlink, I used VARA HF with a Signalink connected to my Kenwood T-120S into a 40m doublet. I enjoyed finding nodes around the regions that I could connect to and mostly sent emails to myself. As I got bored with this, I wanted to explore more mobile operations using my Kenwood TM-V17A in my vehicle as well as man-portable. I purchased a [Digirig](https://digirig.net/) with the requisite cables and successfully connected to a relatively nearby node using my home 2m/70cm vertical on 2m over VARA FM. 

So far, most of this experimentation had been in my shack on my Windows computer. To fully realize my portable Winlink dreams, I was going to need to solve a few problems:
- I needed a device to act as the TNC to connect to the radio
- I needed a device to run Winlink compatible software
- I needed a reliable connection to a Winlink node

# TL;DR
[Dire Wolf](https://github.com/wb2osz/direwolf) is the best TNC software available and is commonly used to decode/encode APRS. It has the capability to be a digitpeater but this feature is not well documented. The [User Guide](https://github.com/wb2osz/direwolf/blob/dev/doc/User-Guide.pdf) mainly discusses APRS digipeaters which are not useful in this context. What is interesting to us is the CDIGIPEAT command is section 10.3.1 of the user guide. The simplest configuration would be modify the generic configuration file with the following changes:
```
MYCALL N0CALL-1
CDIGIPEAT  0   0
```
The MYCALL command is used to specify the callsign and SSID of the node, in this example I am using an SSID of 1 to signifiy that this is the first digipeater using this callsign. Replace N0CALL with your callsign.

The CDIGIPEAT command is the Connected Mode Digipeater followed by the from channel and to channel. In this case both channels are the default '0' channel that correlates to the audio interface connected to the radio.

This configuration will result in all packets addressed to N0CALL-1 being digipeated on the same frequency that it is received on. To use this, set your radio to the same frequency of a reachable AX.25 packet Winlink node and start direwolf. On another radio using a Winlink client, specify that you want to connect the aforementioned Winlink node via N0CALL-1 and attempt to connect. You should hear all packet you send be repeated by your digipeater as well as all packets send from the Winlink node.