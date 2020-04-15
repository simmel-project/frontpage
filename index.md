## Simmel

Simmel is a platform that enables COVID-19 contact tracing while
preserving user privacy. It is a wearable hardware beacon and scanner
which can broadcast and record randomized user IDs. Contacts are
stored within the wearable device, so you retain full control of your
trace history until you choose to share it.

The Simmel design is open source, so you are empowered to audit the
code. Furthermore, once the pandemic is over, you are able to recycle,
re-use, or securely destroy the device, thanks to the availability
of hardware and firmware design source.

The contact tracing algorithm is programmed using CircuitPython, to
facilitate ease of code audit and community participation. The Simmel
project does not endorse a specific contact tracing platform, but it
is inherently not compatible with contact tracing proposals that
rely on the constant upload of data to the cloud.

### Hardware Summary

Simmel is being developed on a very short timeframe. We have two
paths being explored in parallel.

- The **BLE** variant facilitates contact tracing via BLE.
- The **NUS** variant facilitates contact tracing via near ultrasound (NUS).

BLE is a proven, battle-tested technology with mature chipsets and
protocol stacks. Thus it is plausible that a solution could be
deployed at nation-state scales in a matter of months. However, BLE
beacon signal strength is known to be a poor proxy for
locality. Furthermore, running a BLE chipset constantly in scan mode
is a relatively power hungry exercise, driving up battery requirements.

NUS is a novel proposal which relies on near ultrasound (18-22kHz)
acoustically modulated transmissions to determine
proximity. Ultrasound is fairly directional and does not penetrate
walls, so it stands to reason that a signal detected on NUS is a good
indicator of direct physical proximity. Furthermore, the power cost of
sampling a microphone is less than that of BLE scanning. However,
there is no established NUS protocol. Furthermore, NUS requires
transducers that are expensive compared to BLE parts, and also
requires the user to directly wear the device; we are assuming that
e.g. a typical handbag would block the NUS signal. Thus while NUS may
be preferable from a privacy and power standpoint, it is handicapped
by a lack of maturity and higher cost. 

Simmel provisions a 2MiB on-device SPINOR. We anticipate a contact record
to consist of about 96 bytes, which should allow for about 20,000 contacts
to be recorded in a circular buffer that expires over a period of 2-3 weeks
(as set by the policy of the contact tracing application). 


### Firmware Summary

Simmel will run a port of Circuitpython. Circuitpython was chosen oven
an embedded C RTOS for the following reasons:

- The contact tracing algorithm should be simple enough that it can be
  implemented in Circuitpython
- Circuitpython has a lower barrier of entry compared to embedded C
- Code distribution can be done in source form. Provisioning and update
  is a matter of plugging the Simmel device into a USB port and copying
  the source file into an emulated mass storage device.
- Circuitpython has a baseline port to our BLE chipset
- Simmel project will fill in the missing drivers, power management, and
  AES primitives

Because of the open and easy nature of firmware update for Simmel,
once the COVID-19 outbreak is over, users can re-use their Simmel devices
for other applications. It is also easy to provision a script that
securely erases the contact tracing ROM, allowing the devices to be
donated to schools or other organizations for extended use after
the outbreak.


### Application Code

Simmel does not endorse a specific contact tracing algorithm at this
time; however, we are basing many of the system design assumption
around support for protocols that are similar to
[Bluetrace](https://bluetrace.io/).


### About the Simmel project

The Simmel project was started on April 10, 2020 in response to a
request for a hardware design proposal by NLNet. The simmel-project
github repository's people page [lists the
developers](https://github.com/orgs/simmel-project/people) that have
[elected to reveal their participation
publicly](https://help.github.com/en/articles/publicizing-or-hiding-organization-membership).

The administrative contact for the Simmel project is [Andrew
'bunnie' Huang](https://en.wikipedia.org/wiki/Andrew_Huang_(hacker))
([@bunniestudios](https://twitter.com/bunniestudios)/[blog](https://bunniestudios.com)).

<center><img src="https://nlnet.nl/logo/banner.png" width="20%"> <img src="https://nlnet.nl/image/logos/NGI0_tag.png" width="20%"></center>

The Simmel team is funded through the [NGI0 PET
Fund](https://nlnet.nl/PET), a fund established by NLnet with
financial support from the European Commission's [Next Generation
Internet](https://ngi.eu/) programme, under the aegis of DG
Communications Networks, Content and Technology under grant agreement
No 825310.
