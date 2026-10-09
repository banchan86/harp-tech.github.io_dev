# Communication Protocol

The Harp standard has two parts: a communication protocol that allows devices and controllers to speak the same language, and a [synchronization clock](harp-synchronization-clock.md) protocol that governs how devices in a setup keep the same time. This article covers the communication protocol.

A Harp device is controlled by a computer, which we call the controller. The device keeps its settings and readings in numbered addresses called **registers**. Every exchange between the two is a short message that names a register and carries a value, called the **payload**.

::: harp-figure
![Harp messages between a controller and a device](~/images/harp-messages.svg)

Commands go from the controller to the device, and the device answers each one with a reply. Events come from the device on its own.
:::

There are two kinds of command:

- **Write** sets a register to a new value, for example a sound card device might have a register that start sound playback.
- **Read** asks for the current value of a register, for example what is the state of the digital inputs.

The device answers every command with a reply. The reply names the same register and carries its value after the command was handled, plus the time on the device's clock at that moment.

::: harp-figure
![Two commands and their replies](~/images/harp-command-reply.svg)

A Write to the StartSound register carries the sound index to play, and a Read of the DigitalInputs register only names the register. In both cases the device replies with the same register, its current value, and the timestamp from its own clock.
:::

The third kind of message is an **Event**, which the device sends on its own when something happens, for example when an input changes. Events carry a timestamp in the same way as replies, so the timing of everything that happens is recorded on the device rather than on the computer.

For the byte-level details, see the [Binary Protocol](../protocol/BinaryProtocol-8bit.md) specification.
