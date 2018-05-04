## Welcome to MUD Github page

### Introduction

The Internet has largely been constructed for general purpose
computers, those devices that may be used for a purpose that is
specified by those who own the device.  [RFC1984] presumed that an
end device would be most capable of protecting itself.  This made
sense when the typical device was a workstation or a mainframe, and
it continues to make sense for general purpose computing devices
today, including laptops, smart phones, and tablets.

[RFC7452] discusses design patterns for, and poses questions about,
smart objects.  Let us then posit a group of objects that are
specifically not general purpose computers.  These devices, which
this memo refers to as Things, have a specific purpose.  By
definition, therefore, all other uses are not intended.  The
combination of these two statements can be restated as a manufacturer
usage description (MUD) that can be applied at various points within
a network.

We use the notion of "manufacturer" loosely in this context to refer
to the entity or organization that will state how a device is
intended to be used.  For example, in the context of a lightbulb,
this might indeed be the lightbulb manufacturer.  In the context of a
smarter device that has a built in Linux stack, it might be an
integrator of that device.  The key points are that the device itself
is assumed to serve a limited purpose, and that there may exist an
organization in the supply chain of that device that will take
responsibility for informing the network about that purpose.

The intent of MUD is to provide the following:

- Substantially reduce the threat surface on a device entering a
  network to those communications intended by the manufacturer.

- Provide a means to scale network policies to the ever-increasing
  number of types of devices in the network.

- Provide a means to address at least some vulnerabilities in a way
  that is faster than the time it might take to update systems.
  This will be particularly true for systems that are no longer
  supported by their manufacturer.
- Keep the cost of implementation of such a system to the bare
  minimum.

- Provide a means of extensibility for manufacturers to express
  other device capabilities or requirements.

MUD consists of three architectural building blocks:

- A URL that is can be used to locate a description;

- The description itself, including how it is interpreted, and;

- A means for local network management systems to retrieve the
description.

In this specification we describe each of these building blocks and
how they are intended to be used together.  However, they may also be
used separately, independent of this specification, by local
deployments for their own purposes.

#### What MUD Doesn't Do

MUD is not intended to address network authorization of general
purpose computers, as their manufacturers cannot envision a specific
communication pattern to describe.  In addition, even those devices
that have a single or small number of uses might have very broad
communication patterns.  MUD on its own is not for them either.

Although MUD can provide network administrators with some additional
protection when device vulnerabilities exist, it will never replace
the need for manufacturers to patch vulnerabilities.

Finally, no matter what the manufacturer specifies in a MUD file,
these are not directives, but suggestions.  How they are instantiated
locally will depend on many factors and will be ultimately up to the
local network administrator, who must decide what is appropriate in a
given circumstances.

#### A Simple Example

A light bulb is intended to light a room.  It may be remotely
controlled through the network, and it may make use of a rendezvous
service of some form that an application on a smart phone.  What we
can say about that light bulb, then, is that all other network access
is unwanted.  It will not contact a news service, nor speak to the
refrigerator, and it has no need of a printer or other devices.  It
has no social networking friends.  Therefore, an access list applied
to it that states that it will only connect to the single rendezvous
service will not impede the light bulb in performing its function,
while at the same time allowing the network to provide both it and
other devices an additional layer of protection.

#### Terminology

MUD:  manufacturer usage description.

- MUD file:  a file containing YANG-based JSON that describes a Thing
  and associated suggested specific network behavior.

- MUD file server:  a web server that hosts a MUD file.

- MUD controller:  the system that requests and receives the MUD file
  from the MUD server.  After it has processed a MUD file, it may
  direct changes to relevant network elements.

- MUD URL:  a URL that can be used by the MUD controller to receive the
  MUD file.

- Thing:  the device emitting a MUD URL.

- Manufacturer:  the entity that configures the Thing to emit the MUD
  URL and the one who asserts a recommendation in a MUD file.  The
  manufacturer might not always be the entity that constructs a
  Thing.  It could, for instance, be a systems integrator, or even a
  component provider.

#### Determining Intended Use

The notion of intended use is in itself not new.  Network
administrators apply access lists every day to allow for only such
use.  This notion of white listing was well described by Chapman and
Zwicky in [FW95].  Profiling systems that make use of heuristics to
identify types of systems have existed for years as well.

A Thing could just as easily tell the network what sort of access it
requires without going into what sort of system it is.  This would,
in effect, be the converse of [RFC7488].  In seeking a general
purpose solution, however, we assume that a device has so few
capabilities that it will implement the least necessary capabilities
to function properly.  This is a basic economic constraint.  Unless
the network would refuse access to such a device, its developers
would have no reason to provide the network any information.  To
date, such an assertion has held true.

#### Finding A Policy: The MUD URL

Our work begins with the device emitting a Universal Resource Locator
(URL) [RFC3986].  This URL serves both to classify the device type
and to provide a means to locate a policy file.

MUD URLs MUST use the HTTPS scheme [RFC7230].

In this memo three means are defined to emit the MUD URL, as follows:

- A DHCP option[RFC2131],[RFC3315] that the DHCP client uses to
inform the DHCP server.  The DHCP server may take further actions,
such as retrieve the URL or otherwise pass it along to network
management system or controller.

- An X.509 constraint.  The IEEE has developed [IEEE8021AR] that
provides a certificate-based approach to communicate device
characteristics, which itself relies on [RFC5280].  The MUD URL
extension is non-critical, as required by IEEE 802.1AR.  Various
means may be used to communicate that certificate, including
Tunnel Extensible Authentication Protocol (TEAP) [RFC7170].

- Finally, a Link Layer Discovery Protocol (LLDP) frame is defined
[IEEE8021AB].




#### Source 

[draft-ietf-opsawg-mud](https://datatracker.ietf.org/doc/draft-ietf-opsawg-mud/)
