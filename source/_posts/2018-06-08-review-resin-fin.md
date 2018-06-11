---
layout: post
title:  "Resin Fin - Initial Thoughts and Impressions"
date:   2018-06-09 13:46:52
comments: true
image: /images/resin-fin.jpg
categories: review resin
---

I've been lucky enough to get my hands on an early production version of the [Resin Fin](https://resin.io/blog/introducing-project-fin-a-board-for-fleet-owners/); I'm working on a gateway-like platform for a client. I was intrigued when I first heard that Resin was making their own hardware as industrialised IoT platforms, especially those using the Raspberry Pi (RPi) Compute Module, are few and far between. 

Resin has included some interesting features with the Fin, including the addition of a Samsung Artik 020 co-processor as well as a mini PCI-E expansion socket (with nano-sim reader). Although I've had the Fin for a couple of weeks now, I will likely follow up this post with more details when Resin releases their documentation for the device as there are still some pre-production complexities with software and set up etc.

#### What is Resin?

For those who are reading this initial impressions post and are unfamiliar with the [Resin](https://resin.io/) platform, Resin is a scalable platform for managing fleets of IoT devices. It's built on top of a containerized Linux distribution that runs on deployed devices (think Docker for IoT). This allows you to remotely implement version control, monitoring and provisioning across a fleet of devices whilst maintaining a high level of reliability, inherent from the containerization. As all the application that you deploy are loaded into containers, you can maintain a code base across a range of different hardware so long as it has support for [Resin OS](https://resinos.io/). Additionally, this means that devices are virtually safe from being bricked in the field as applications are restricted to their containers. 

Although it's a little bit more complicated than shown in the image below, it gives a good generalised understanding of the steps involved between deploying code and the loop between hardware and the programmer.

<div style="text-align:center">
<img src="/images/resin-flow.jpeg" width="600px">
<h5 align="center">Resin Workflow (credit - Resin.io)</h5>
</div>

The Fin is Resin's own take on what the hardware that powers their containerized Linux operating system should look like. Typically, many of their examples and documentation is demonstrated on Raspberry Pi B hardware, which while great for cheap and simple deployments, often isn't fit for production scenarios.

## The Hardware

From a hardware perspective, the Resin Fin offers a lot of interesting technology with a particular focus on gateway-esq features. It's clear from the Fin's offering, that Resin sees this platform as an enabler of dependant devices (think microcontrollers that use the Fin as a communication relay). By default, the Fin offers Dual-band 2.4 & 5 GHz support as well as Bluetooth 4.2. 

#### Form Factor

The Fin has a square profile of 90 mm by 90 mm with a height of approx. 25 mm with compute module attached. I was surprised when I first unpackaged the device as it was considerably smaller than the impression given by Resin's blog post.

I understand that Resin will be offering some of their own cases, with variants of DIN and VESA mounts. I look forward to seeing the quality of their offering but ideally would like to see something with IP rated water proofing. It'd be ideal to mount a Fin onto an external wall or pole, exposed to weather conditions, to support outside deployments.

*Update - the [Resin Store](https://store.resin.io) now has listings for the VESA and DIN cases as well as the Fins devices.*

<div style="text-align:center">
<img src="/images/resin-fin-001.JPG" width="400px">
<h5 align="center">Top View</h5>
</div>

#### Power Management

The Resin Fin is an industrialised RPi Compute platform and this is reflected in Resin's choice of power delivery. The Fin can take in between 6 V to 30 V on either a barrel jack connector or phoenix connector, making it suitable for the larger voltages of the kinds of systems you might find on a DIN rail (e.g. a standard PLC requires 24 V DC).

Resin suggests a couple of features for power conservation such as using the Artik Co-Processor for powering down the Compute Module as well as providing some commands to power down various radio interfaces. Although an interesting features, I personally see the Fin as an always on system more suited for gateway and high performance activities; I'll touch more on this later.

<div style="text-align:center">
<img src="/images/resin-fin-004.JPG" width="400px">
<h5 align="center">Power Connectors</h5>
</div>


Note - <i style="color:#28BC2D">although the Fin has a micro USB port, you cannot use this to power the device as it is for debugging purposes.</i>

#### I/O and Expansion

The Fin has both the standard selection of I/O that you'd expect from a typical RPi device as well as a few special features too. As you'd expect, it has a 3.5mm output jack, the iconic *RPi HAT connector*, an *HDMI port*, 2x *USB 2.0 ports* as well as *display and camera connectors*. The Fin provides a user customised RGB LED, which I've found useful for denoting the device's networking status (Red for disconnected, blue for WiFi, Green for Ethernet, etc.) as well as user control over all the status LEDs on the board.

#### Compute Module 3 Lite

The Compute Module 3 Lite (CM3L) is a nice feature for a device such as the resin as it provides an increased longevity of support as the CM3L can be swapped out for new modules as and when the Raspberry Pi Foundation releases them. The reason that Resin has chosen to use the Lite is that it means they can offer different variations of the Fin with support for 8 GB to 64 GB of eMMC 5.1 Flash Memory, depending on the user's use case. The CM3L uses the BCM2837, the same Broadcom chip found in the RPi 3, which is a quad core ARMv8 cluster, clocked at 1.2 GHz with its GPU clocked at 400 MHz.  

<div style="text-align:center">
<img src="/images/resin-fin-002.JPG" width="400px">
<h5 align="center">Rear View (CM3L)</h5>
</div>

#### Artik 020 Co-Processor

The [Artik 020](https://www.artik.io/modules/artik-020/) co-processor is an interesting addition to the Fin as it allows for the entire compute module to be powered down and up, programmatically. The Artik is also on the same I2C bus as the Fin's Real Time Clock (RTC) and can use this to track time that the compute module is set to sleep for. 

I'm yet to use this in an application but I could see this being useful if using the Fin in a power considerate situation such as relying on a battery + solar power solution. It might also find use in using the Artik's onboard Bluetooth module for detecting the presence of external devices and using this as an interrupt for the Fin's power down cycle.

Resin has mentioned that they are implementing firmata support for the Artik to allow it to be controlled via a client library on the CM3L. This would make the process of it much more simple as all of the control could sit within the Resin OS user application container, rather than having to reprogram the Artik every time.

<div style="text-align:center">
<img src="/images/resin-fin-006.JPG" width="400px">
<h5 align="center">Artik 020 Co-Processor</h5>
</div>

Although I can see it having uses, I don't particularity envision the resin as a device that would really utilise a sleep functionality; to me it's better suited as an always on hub for relaying information back up to the cloud. Resin OS is most powerful when it can be updated immediately and instantly to any user changes/modifications. In my mind resin devices are better as edge compute platforms rather than data aggregation which is better left to dependent devices like microcontrollers, which might use a Resin device as a back haul for their own telemetry.

#### Networking

As mentioned earlier, the Resin Fin has the standard offering of Dual-band 802.11ac/a/b/g/n with both 2.4 & 5GHz support as well as Bluetooth 4.2. I appreciate that they also provide a uFL connector for the WiFi/Bluetooth radio; you might expect the Fin to be crammed into a PLC shelf or next to RF noisy equipment. 

<div style="text-align:center">
<img src="/images/resin-fin-003.JPG" width="400px">
<h5 align="center">Rear View (CM3L + Mini PCI-E Modem)</h5>
</div>

I've included a Huawei ME909 Cellular Modem (Mini PCI-E) with this blog post as cellular connectivity is an important part of the Fin's offering. 

I was impressed by how easy it was to get up and running with this modem. As soon as I had added the config for Network Manager, the Fin immediately picked it up and started transmitting data. I've included the config file below:

{% highlight bash %}[connection]
id=resin-cellular
type=gsm
autoconnect=true

[gsm]
apn=hologram
number=*99#

[serial]
baud=115200

[ipv4]
method=auto

[ipv6]
addr-gen-mode=stable-privacy
method=ignore

{% endhighlight %}

Be cautious that this Network Manager config will immediately fire up the Cellular Modem and start transmitting all your data over 3G/4G. This will spend your data allowance pretty quickly if you're not careful!

## The Software

#### The Setup Process

I'm cautious to comment on this as I understand that the Fin is still in development and that the Software/Set up Guides are not yet publicly exposed. I did find this somewhat convoluted and the instructions to be a little sparse but again, I have a pre-production device so this is to be expected!

To set the Fin up, I had to put the device into debug mode which exposes the boot partition of the compute module. To do this on Linux (Debian), I had to install the Raspberry Pi Foundation "usbboot" tool which is designed to access the bootloader of the Compute Modules. 

#### Resin OS

I won't touch on too much detail here as I briefly explained what Resin is and how it works in the introduction - which in its own doesn't really do justice to completely describing how Resin works!

Although intended for use with Resin OS, there's nothing dictating that this hardware can only use Resin OS; you're more than welcome to put Raspbian, Ubuntu Mate or any other RPi distribution of your choice on there!

<div style="text-align:center">
<img src="/images/resin-io-001.png" width="600px">
<h5 align="center">My Application - Resin.io</h5>
</div>

What's nice about resin is that all it takes is a simple git push and you can iterate on the version of code running on your device:

{% highlight bash %}$ git add . 
$ git commit -m "Here goes nothing!" 
$ git push resin master
{% endhighlight %} 

And your code is pushed/built to a docker container at Resin.io before being forward onto your Resin OS device (if the image builds and deploys successfully).

## My Thoughts & Impressions

Overall, I'm very impressed with the Fin and I think Resin have done a great job positioning its features and use case towards a clear gap in the market. The Fin will inevitably find its way to the maker audience but I think it'll find a nice home in many industrial use cases. Combining the Fin's ability to accept mini-PCIE radio interfaces (LoRaWAN, LTE, Zigbee, etc.) with Resin's powerful Resin OS platform means that the Fin is can become a very capable gateway.  