## Simmel

Simmel is a platform that enables COVID-19 contact tracing while
preserving user privacy. It is a wearable hardware beacon and scanner
which can broadcast and record randomized user IDs. Contacts are stored
within the wearable device, so you retain full control of your trace
history until you choose to share it.

![Simmel diagram](https://github.com/simmel-project/frontpage/raw/master/simmel-concept.png)

The Simmel design is open source, so you are empowered to audit the
code. Furthermore, once the pandemic is over, you are able to recycle,
re-use, or securely destroy the device, thanks to the availability of
hardware and firmware design source.

The contact tracing algorithm is programmed using CircuitPython, to
facilitate ease of code audit and community participation. The Simmel
project does not endorse a specific contact tracing platform, but it is
inherently not compatible with contact tracing proposals that rely on
the constant upload of data to the cloud.

### Hardware Summary
The hardware design source for Simmel can be found [here](https://github.com/simmel-project/hardware). The EVT version is based around the [Nordic NRF52833](https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52833).

![Simmel overview](https://github.com/simmel-project/frontpage/raw/master/simmel-desk.jpg)

Simmel is being developed on a very short timeframe. We have two paths
being explored in parallel.

- The **BLE** variant facilitates contact tracing via BLE.
- The **NUS** variant facilitates contact tracing via near ultrasound
  (NUS).

BLE is a proven, battle-tested technology with mature chipsets and
protocol stacks. Thus it is plausible that a solution could be deployed
at nation-state scales in a matter of months. However, BLE beacon signal
strength is known to be a poor proxy for locality. Furthermore, running
a BLE chipset constantly in scan mode is a relatively power hungry
exercise, driving up battery requirements.

NUS is a novel proposal which relies on near ultrasound (18-22kHz)
acoustically modulated transmissions to determine proximity. Ultrasound
is fairly directional and does not penetrate walls, so it stands to
reason that a signal detected on NUS is a good indicator of direct
physical proximity. Furthermore, the power cost of sampling a microphone
is less than that of BLE scanning. However, there is no established NUS
protocol. Furthermore, NUS requires transducers that are expensive
compared to BLE parts, and also requires the user to directly wear the
device; we are assuming that e.g. a typical handbag would block the NUS
signal. Thus while NUS may be preferable from a privacy and power
standpoint, it is handicapped by a lack of maturity and higher cost.

Simmel provisions a 2MiB on-device SPINOR. We anticipate a contact
record to consist of about 96 bytes, which should allow for about 20,000
contacts to be recorded in a circular buffer that expires over a period
of 2-3 weeks (as set by the policy of the contact tracing application).

![Simmel rear view](https://github.com/simmel-project/frontpage/raw/master/simmel-rear.jpg)

### Firmware Summary

Simmel will run a port of CircuitPython. CircuitPython  was chosen oven
an embedded C RTOS for the following reasons:

- The contact tracing algorithm should be simple enough that it can be
  implemented in CircuitPython
- CircuitPython  has a lower barrier of entry compared to embedded C
- Code distribution can be done in source form. Provisioning and update
  is a matter of plugging the Simmel device into a USB port and copying
  the source file into an emulated mass storage device.
- CircuitPython has a baseline port to our BLE chipset family
- Simmel project will fill in the missing drivers, power management, and
  cryptographic primitives

Because of the open and easy nature of firmware update for Simmel,
once the COVID-19 outbreak is over, users can re-use their Simmel devices
for other applications. It is also easy to provision a script that
securely erases the contact tracing ROM, allowing the devices to be
donated to schools or other organizations for extended use after
the outbreak.

![Simmel USB connector](https://raw.githubusercontent.com/simmel-project/frontpage/master/simmel-usb.jpg)

### Application Code

Simmel does not endorse a specific contact tracing algorithm at this
time; however, we are basing many of the system design assumption around
support for protocols that are similar to
[Bluetrace](https://bluetrace.io/).  The Simmel hardware does not
possess the ability to connect directly to the Internet, and is
therefore incapable of sharing its logging data without user
intervention.

### Media

Below are some images of the initial Simmel prototype as fabricated
using a 3D printer.

![Simmel prototype](https://github.com/simmel-project/frontpage/raw/master/simmel_pen_compare.jpg)

In addition to being packed into a backpack, purse or briefcase, the
simmel device can be hung from a lanyard.

![Simmel lanyard](https://github.com/simmel-project/frontpage/raw/master/simmel_assembled.jpg)

![Simmel as worn](https://github.com/simmel-project/frontpage/raw/master/simmel_lanyard.jpg)

Here's a photo of Simmel as plugged into a USB type C socket on a typical laptop.

![Simmel plugged in](https://github.com/simmel-project/frontpage/raw/master/simmel_plugged_in.jpg)

And here are some photos of the Simmel PCBA.

![Simmel PCBA](https://github.com/simmel-project/frontpage/raw/master/simmel_pcb_quarter1.jpg)

![Simmel PCBA](https://github.com/simmel-project/frontpage/raw/master/simmel_pcb_sideview.jpg)

### About the Simmel Project

The Simmel project was started on April 10, 2020 in response to a
request for a hardware design proposal by NLNet. The simmel-project
GitHub repository's people page [lists the
developers](https://github.com/orgs/simmel-project/people) that have
[elected to reveal their participation
publicly](https://help.github.com/en/articles/publicizing-or-hiding-organization-membership).

The Simmel project name comes from [Georg
Simmel](https://wikipedia.org/wiki/Georg_Simmel), an early researcher
into sociology and social distance theory.

The administrative contacts for the Simmel project are:

* [Andrew 'bunnie'
  Huang](https://en.wikipedia.org/wiki/Andrew_Huang_(hacker))
  ([@bunniestudios](https://twitter.com/bunniestudios)/[blog](https://bunniestudios.com))
* [Sean 'xobs' Cross](https://xobs.io)

---

<center><img src="https://nlnet.nl/logo/banner.png" width="20%"> <img src="https://nlnet.nl/image/logos/NGI0_tag.png" width="20%"></center>

The Simmel team is funded through the [NGI0 PET
Fund](https://nlnet.nl/PET), a fund established by NLnet with financial
support from the European Commission's [Next Generation
Internet](https://ngi.eu/) programme, under the aegis of DG
Communications Networks, Content and Technology under grant agreement No
825310.
