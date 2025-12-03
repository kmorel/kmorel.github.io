---
title: "TV With a Side of MoCA"
date: 2015-04-03
tags: [ electronics, TiVo, MoCA ]
summary: "A description of using MoCA to use existing coax cable to implement wired ethernet without pulling new cat cables."
---


This winter I decided to consolidate my TiVo DVR service to a single
[Roamio] that has one subscription and can stream a show to other rooms
over the home network. The service is a lot nicer, but there is a catch.
The TiVo units will not let you do this over a WiFi network, which is what
I (and many households) use to connect network devices. Apparently, the
TiVo engineers have deemed WiFi networks too unreliable to stream HD video
signals, so that means I have to find a method to wire the devices across
my house.

One simple way around this problem is to use wireless bridge adapters,
which are simple devices that take a wired Ethernet connection and pass it
to a WiFi network. That will make the TiVo think it is on a wired
connection, but ultimately the data is still transmitted wirelessly. I
trust that there are valid reasons TiVo does not want to use wireless
networks with its Roamio streaming, so I suspect this approach will yield
at least intermittent problems. I did not try it.

Another straightforward approach would be to add wired Ethernet by running
cat6 wires throughout the house. It can be a bit of work to run the wires,
but not too expensive. Unfortunately, my house has a flat roof with no
crawlspace, so adding new wiring generally means adding exposed wires
running along the inside or outside walls.

Fortunately, TiVo provides a third option that, for me, is even cheaper and
easier. My TiVo units support a network technology named [MoCA], pronounced
"mocha" like the coffee drink and which stands for Multimedia over Coax. It
uses the same coaxial cable that sends the TV signal throughout the house
and runs a network signal through it as well.

The MoCA network turned out to be a cheap, easy, and effective means to
establish a wired network through my house. Although I found several web
sites with information on setting up a MoCA network including [TiVo's own
information], I found very little with respect to practical experience and
possible pitfalls. On that note, I am writing this blog outlining the steps
I followed to establish the TiVo/MoCA network in hopes that other folks
considering the approach will find it useful. This post is written
pedantically, which might make for dry reading but will hopefully provide
answers to questions not found elsewhere.

[Roamio]: https://www.tivo.com/shop/roamio
[MoCA]: http://www.mocalliance.org/
[TiVo's own information]: https://www.tivo.com/my-account/how-to/what-moca


## Map Your Cables

I advise to start your MoCA setup by first mapping out the existing cabling
in your residence. This will help identify any potential problems you might
run into as well as ascertain the viability of setting up the MoCA network
in the first place. As an example, here is the layout of the cables in my
house (diagram not to scale).

![](cable-map.svg)

Your mapping probably does not have to be as detailed as this, but you
should at least know where the cable signal comes in and through what wires
and components the MoCA devices will communicate. In particular, you should
be familiar with the following features of your wires.

### Point of Entry

{{< right >}}
{{< figure
link="cable-box-closed.jpg"
thumb="-thumbnail"
alt="Cable Utility Box (closed)"
>}}
{{< /right >}}

The typical cable installation in a house is to have a single line coming
into the house, which is eventually distributed among the rooms in the
house. This place where the cable comes into your house, typically either
from a buried cable or line running from a utility pole, is called the
"point of entry." Typically the point of entry is in a plastic utility box
mounted to the side of the building. At right is a picture of the box
mounted to the side of my house.

{{< left >}}
{{< figure
link="cable-box-original.jpg"
thumb="-thumbnail"
alt="Cable Utility Box (open)"
>}}
{{< /left >}}

If you have identified your point of entry as one of these utility boxes,
it is a good idea to open it up and have a look inside. Now, I know that
opening up utility boxes can be scary, and for some utility boxes, like a
circuit breaker box, it can be downright dangerous. But as you can see from
the picture of the inside of my box at left, a cable utility box typically
just contains some coaxial cables and a splitter, which is the same stuff
you might use to hook up your entertainment center. (Let it be stated
though, that you perform this and any other actions _at your own risk_. I
do not take any responsibility for harm to yourself or equipment.)

If you do not have a single point of entry or you cannot find it, there may
be problems getting a MoCA network to work throughout all the cable in your
residence. For example, if you live in an apartment complex where the
cabling is shared among apartments and their are multiple entry points
into your apartment, it may not be possible to send the MoCA network from
one entry point to the next.

### Splitters

Assuming your residence has a single point of entry, the typical cable
installation takes that input and distributes it to different rooms through
the use of one or more devices called splitters. A splitter is a small box,
typically no more than a few inches in each dimension, that has one input
coaxial connector and 2 or more output connectors. As the name implies,
the device splits the input signal evenly to each output.

{{< right >}}
{{< figure
link="splitter.jpg"
thumb="-thumbnail"
alt="Cable splitter"
>}}
{{< /right >}}

All the splitters that the cable company installed in my house looked like
this device pictured to the left. These splitters happen to split the
signal in 2, but 3- and 4-way splitters are common, and I have seen up to
16-way splitters.

I suggest following your coaxial cables throughout your residence to find
any and all splitters. This helps you trace the topology of your wiring,
which could be helpful. But more importantly, as described later there is a
good chance you will have to replace all the splitters in your house, so
make sure there are none hiding in walls or crawlspaces.

While we are on the subject of splitters, I want to mention that there is a
similar device called a tap. Taps look the same as splitters and have a
similar function. Taps are used to break many connections off a long
running wire. They are used to provide many drop points off a wire that
runs through a large structure. This might be the setup used to run a
signal to many rooms in an apartment complex or a school building. You are
unlikely to run into a tap inside your dwelling, so you probably don't have
to worry about taps. Just be sure you do not accidentally buy a tap if you
need to replace a splitter.

### Amplifiers and other equipment

{{< right >}}
{{< figure
link="amplifier.jpg"
thumb="-thumbnail"
alt="Cable amplifier"
>}}
{{< /right >}}

As you trace the cables through your house, you may encounter other
equipment that is not a splitter. For example, in my house near the den TV
there was an amplifier installed in the line (shown to the right).
Equipment like this is problematic because it is usually not designed to
let network signals pass through in both directions.

If you run into equipment like this, it is probably best to assume that it
will block any network signal. You will have to route the network around
the equipment somehow.

{{< left >}}
{{< figure
link="coupler.jpg"
thumb="-thumbnail"
alt="Coupled cables"
>}}
{{< /left >}}

In the specific case of my amplifier, I simply removed it and replaced it
with a coupler that joins the two wires. The amplifier was originally
installed by a cable technician to improve the reception back when I had
analog service. However, having an amplifier right next to the receiving
equipment is of dubious value, particularly for a digital signal.
(Amplifiers are more useful at the point of entry to boost the signal
before it degrades.) At any rate, I no longer needed the amplifier.
Everything works fine without it.


## Check Your Equipment Rating

Now that you have mapped out the wiring of your cable installation, trace
the path between the points at which you will install the MoCA equipment
(for example, the TiVo Roamio unit and any TiVo Mini it will communicate
with). The path will almost certainly pass through one or more splitters.
You need to make sure that any splitters or other equipment in the path is
MoCA compatible.

The first requirement for a MoCA network is that the equipment is
bidirectional. That is, you can send a signal from output to input as well
as input to output. Some splitters are explicitly marked as bidirectional
or splitter/combiners, but others may work, too. I saw a claim on a message
board that "all splitters rated 5&ndash;1000 MHz are bidirectional." I'm
not sure that is actually true, but I suspect most passive splitters can
function bidirectionally. Active components like amplifiers need to be
explicitly designed as bidirectional or they will not work.

The second requirement is that the equipment needs to pass the MoCA signal,
which is at about 2 GHz, without too much dampening. Most of the splitters
that I have run into are rated up to about 1000 MHz (a.k.a. 1 GHz). That
means the MoCA signal will be dampened quite a bit.

MoCA equipment is designed to be tolerant of this dampening by sending out
a strong signal and being able to read weak signals. Thus, if your path
involves only one splitter, the MoCA network will probably still work even
if it is not rated for the MoCA signal.

{{< right >}}
{{< figure
link="moca-compatible-splitter.jpg"
thumb="-thumbnail"
alt="MoCA compatible splitter"
>}}
{{< /right >}}

Unfortunately for me, the cable installation of my house uses several
splitters. The path between my Roamio and Mini units runs through 4
splitters, so I have to worry about signal degradation. The original
splitters were only rated for up to 1000 MHz, so I replaced them with the
splitters shown to the right, which are rated for up to 3 GHz. Finding
splitters rated like this is not difficult and they are not expensive. I
honestly never even tried to use the original splitters because I was
trying to avoid problems. I cannot say whether the original splitters would
have worked, but I can attest that the new splitters are working just fine.


## Install a POE Filter

Once all the equipment inside your dwelling is compatible with MoCA, there
is one final issue that can occur with a MoCA network. The MoCA network
signal can leak back out into the cable line and possibly into your
neighbor's line. This can cause several minor problems.

1. Driving a signal outside of your local cable network can degrade the
signal inside.

2. The MoCA signal can infiltrate your neighbor's signal. This can cause
problems with their service, especially if they are trying to use a similar
network.

3. Conversely, if your neighbor has their own MoCA network, their signal
can permeate your home and disrupt your network.

4. Allowing your signal to permeate to the open network provides an
opportunity for cyber attackers to infiltrate your network.

{{< right >}}
{{< figure
src="POE-filter-thumbnail.jpg"
link="POE-filter.jpg"
alt="POE filter"
>}}
{{< /right >}}

A simple solution to all of these problems is to install a POE filter,
pictured at right. POE stands for "point of entry," and as the name implies
it is installed at the cable's point of entry we identified earlier. The
POE filter passes the cable TV signal through but blocks your MoCA network
signal from leaking out to the main line as well as block other network
signals from leaking in.

POE filters are cheap and easy to get from retailers like [Amazon]. They
are also easy to install. Below is a picture of the installation in my
cable utility box (along with the installation of a new splitter).

<table class="image-table">
<tr>
<td>
{{< figure
link="cable-box-original.jpg"
thumb="-thumbnail"
width="300px"
alt="Original Cable Utility Box"
caption="Original equipment in cable utility box."
>}}
</td>
<td>
{{< figure
link="cable-box-new.jpg"
thumb="-thumbnail"
width="300px"
alt="Modified Cable Utility Box"
caption="Replaced equipment including POE filter and new splitter."
>}}
</td>
</tr>
</table>

[Amazon]: https://amazon.com


## Final Setup

Once all the MoCA-compatible hardware is in place, it is a simple matter to
[turn on MoCA networking in the TiVo unit]. One final odd behavior I found
was that once you set up a TiVo Roamio to function as a MoCA server, it
will refuse to use WiFi for anything, including simply connecting back to
the TiVo service for updates (which are otherwise fine over WiFi
connections). I could have solved this with a wireless bridge adapter, but
instead I just moved my cable modem into my den close to the TiVo. This
also improved the performance of streaming services as well as the
performance of my [WiFi TV remote].

[turn on MoCA networking in the TiVo unit]: https://explore.tivo.com/how-to/home-network#moca
[WiFi TV remote]: ../tv-remote
