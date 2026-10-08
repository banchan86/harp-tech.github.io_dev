# How Harp Works

The Harp standard has two parts: a communication protocol that allows devices and controllers to speak the same language, and a synchronization clock protocol that governs how device in a setup keep the same time.

## Harp Communication Protocol

A Harp device is controlled by a computer, which we call the controller. The device keeps its settings and readings in numbered slots called registers. Every exchange between the two is a short message that names a register and carries a value, called the payload.

::: harp-figure
![Harp messages between a controller and a device](~/images/light-harp-messages.svg){.display-light}
![Harp messages between a controller and a device](~/images/dark-harp-messages.svg){.display-dark}

Write and Read messages go from the controller to the device. Event messages come from the device on its own.
:::

There are three kinds of message:

- **Write** sets a register to a new value, for example starting a sound.
- **Read** asks for the current value of a register, for example the state of the digital inputs.
- **Event** is sent by the device on its own when something happens, for example when an input changes.

Every Write and Read is a command, and the device answers each one with a reply. The reply names the same register and carries its value after the command was handled, plus the time on the device's clock at that moment.

::: harp-figure
![A Write message and the device reply](~/images/light-harp-write-reply.svg){.display-light}
![A Write message and the device reply](~/images/dark-harp-write-reply.svg){.display-dark}

A Write to the StartSound register at address 32 carries the sound index to play. The device replies with the same register and value, and adds the timestamp from its own clock.
:::

Events carry a timestamp in the same way, so the timing of everything that happens is recorded on the device rather than on the computer.

For the byte-level details, see the [Binary Protocol](../protocol/BinaryProtocol-8bit.md) specification.

## Harp Synchronization Clock

Each device has its own clock, so two devices that saw the same event would normally record it with two unrelated timestamps. Harp solves this with a dedicated clock cable. One device, the Timestamp Generator, broadcasts the current time once per second, and every device connected to it aligns its clock to that time.

::: harp-figure
![Device timelines without and with the synchronization clock](~/images/light-harp-clock.svg){.display-light}
![Device timelines without and with the synchronization clock](~/images/dark-harp-clock.svg){.display-dark}

Without a shared clock the same event is recorded at three unrelated times. With the synchronization clock every device records it at the same time.
:::

Once devices share the clock, their timestamps can be compared directly, with no alignment step afterwards. Devices stay synchronized to within tens of microseconds, and the clock cable is separate from the USB connection, so devices plugged into different computers still share the same time.

For the electrical and timing details, see the [Synchronization Clock](../protocol/SynchronizationClock.md) specification.
