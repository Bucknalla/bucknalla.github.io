---
layout: post
title:  "Initial Impressions - Resin Fin - Industrialised RPi Compute Platform"
date:   2018-06-08 13:46:52
comments: true
image: /images/resin-fin.jpg
categories: review resin
---

I've been lucky enough to get my hands on an early production version of the Resin Fin, for a use in client project, to build a gateway type platform. I was intrigued when I first heard that Resin was making their own hardware as industrialised linux platforms, especially those using the Raspberry Pi (RPi) Compute Module, are few and far between. 

Resin has added some interesting features to the Fin, including the addition of a Samsung Artik 020 co-processor as well as a mini PCI-E expansion socket (with nano-sim reader). I have had the Fin for a couple of weeks at the time of publishing and will likely follow up this post with a full review when Resin releases their official documentation for the device as there are still some pre-production complexities with software, etc.

## The Hardware

From a hardware perspective, the Resin Fin offers a lot of interesting technology with a particular focus on gateway-esq features. It's clear from the Fin's offering, that Resin sees this platform as an enabler of dependant devices (think microcontrollers that use the Fin as a communication relay). By default, the Fin offers Dual-band 2.4 & 5 GHz support as well as Bluetooth 4.2. 

#### Power Management

The Resin Fin is an industrialised RPi Compute platform and this is reflected in Resin's choice of power delivery. The Fin can take in between 6 V to 30 V on either a barrel jack connector or phoenix connector, making it suitable for the larger voltages of the kinds of systems you might find on a DIN rail (e.g. a standard PLC requires 24 V DC).

Additionally, Resin quotes a couple of features for power conservation such as using the Artik Co-Processor for powering down the Compute Module as well as providing some example Bash commands to power down radio interfaces. Although an interesting features, I personally see the Fin as an always on system more suited for gateway and high performance activities.

#### Form Factor

The Fin has a square profile of X mm by Y mm with a height of Z mm with compute module attached. I was surprised when I first unpackaged the device as it was considerably smaller than the impression given by Resin's blog post.

I believe that Resin will be offering some of their own cases, with variants of DIN and VESA mounts. I look forward to seeing the quality of their offering but ideally would like to see something with IP rated water proofing. It'd be fantastic to mount a Fin onto an external wall or pole to support outside deployments.

#### I/O and Expansion

The Fin has both the standard selection of I/O that you'd expect from a typical RPi device as well as a few special features too. As you'd expect, it has a 3.5mm output jack, the iconic RPi HAT connector, an HDMI port, 2x USB 2.0 ports as well as display and camera connectors. The Fin also provides a user customised RGB LED, which I've found useful for denoting the device's networking status (Red for disconnected, blue for WiFi, Green for Ethernet, etc.)

#### Compute Module 3 Lite

The Compute Module 3 Lite (CM3) is a nice feature for a device such as the resin as it provides an increased longevity of support as the CM3 can be swapped out for new modules as and when the Raspberry Pi Foundation releases them. The reason that Resin has chosen to use the Lite is that it means they can offer different variations of the Fin with support for 8 GB to 64 GB of eMMC 5.1 Flash Memory, depending on the user's use case. The CM3 uses the BCM2837, the same Broadcom chip found in the RPi 3, which is a quad core ARMv8 cluster, clocked at 1.2 GHz with its GPU clocked at 400 MHz.  

#### Artik Co-Processor

#### Networking

As mentioned earlier, the Resin Fin has the standard offering of Dual-band 802.11ac/a/b/g/n with both 2.4 & 5GHz support as well as Bluetooth 4.2. I appreciate that they also provide a uFL connector for the WiFi/Bluetooth radio; you might expect the Fin to be crammed into a PLC shelf or next to RF noisy equipment. 

## The Software

#### The Setup Process

I'm cautious to comment on this as I understand that the Fin is still in development and that the Software/Setup Guides are not yet publicly exposed. I did find this somewhat convoluted and the instructions to be a little sparse but again, I have a pre-production device so this is to be expected!

To set the Fin up, I had to put the device into debug mode which exposes the boot partition of the compute module. To do this on Linux (Debian), I had to install the Raspberry Pi Foundation "usbboot" tool which is designed to access the bootloader of the Compute Modules. 

#### Resin OS

## My Impression

Overall, I'm very impressed with the Fin and I think Resin have done a great job positioning its features and use case towards a clear gap in the market. The Fin will inevitably find its way to the maker audience but I think it'll find a nice home in many industrial usecases. Combining the Fin's ability to accept mini-PCIE radio interfaces (LoRaWAN, LTE, Zigbee) with Resin's powerful Resin OS platform means that the Fin is can become a very capable gateway.  