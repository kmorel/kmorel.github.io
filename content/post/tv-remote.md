---
title: "TV Remote? There's an App for That"
date: 2014-11-05
tags: [ hobby, Raspberry Pi, iPhone, remote control ]
summary: "A project using a Raspberry Pi with wired IR emitters to create a server controlling the equipment of a home entertainment center."
---

I lost count of how many times I have sat down to vegetate in the front of
the TV only to discover the remote control missing. The couch eats them and
the kids take them to random locations. And of course, trying to operate a
DVR or DVD player without the remote is not possible, so I spend the next
half hour frantically scouring the house looking for one.

Then it occurred to me that I always know exactly where my iPhone is
(because it's mine and I'm careful with it), and it is usually in my
pocket. Plus, everyone else in the family has their own mobile device that
so far they have not lost. Wouldn't it be nice if we could control our
entertainment center with them?

Well, an iPhone doesn't come with an infrared (IR) emitter, which is what
almost all TV remote controls have used since the 1980's, and my devices
are no exception. So for this idea to work, there needs to be an
intermediate device that can communicate with the iPhone and send IR
signals to the TV and associated devices.

Fortunately, this a great time to homebrew such a product. There are
several hobby devices that are low powered microprocessors with simple
connections to control physical devices. For this project, I chose to use a
[Raspberry Pi], which is a computer not much larger than a credit card that
is network enabled and supports a Linux-based operating system. It also has
several connector pins that can send signals to external devices. With just
a bit of hardware and software, we can set up a TV remote server accessible
to the mobile devices connected to my home network.

{{< youtube id="R7SFihf9yG0" >}}

[Raspberry Pi]: http://www.raspberrypi.org/

## The Hardware

{{< right >}}
{{< figure src="ir-led.jpg" alt="IR LED" width="160px" >}}
{{< /right >}}

It doesn't take much hardware to make a remote control. Really all you are
doing is flashing a light at the TV, which has a sensor to pick up certain
combinations of flashes. You can't see the light because it's outside the
visible range for humans, but otherwise it behaves just the same. IR LEDs
that shine IR light, like the one shown at right, are cheap and easy to
come by. To shine the light, just run electricity through the LED. We will
use the Raspberry Pi computer to flash the light in the right
sequence. More on that later.

Although the Raspberry Pi circuit board has some pins that can send an
electric signal, the current they provide is not strong enough to generate
enough light in the LED. To provide enough current to the LED, we need a
simple circuit to amplify the signal, which is shown in the schematic
below.

![](ir-emitter-circuit.svg)

The principal component in this circuit (other than the IR LED light) is a
transistor (represented by a circular icon in the middle of the
schematic). The transistor behaves essentially like a switch. When a signal
comes from the output pin of the Raspberry Pi (labeled GPIO 23) and runs
through the transistor in one direction, this allows a larger current to
run through the transistor in a different direction and power the IR
LED. There are a couple of small limiting resistors (represented by
squiggly lines) that prevent too strong a current from burning out the
electronics. Finally, a larger "pull down" resistor makes sure the switch
gets shut off when the Raspberry Pi is sending no signal, which can happen.

{{< right >}}
{{< figure src="circuit-board-thumbnail.jpg" width="310px"
link="circuit-board.jpg" alt="circuit board" >}}
{{< /right >}}

Here is the circuit board I soldered up. The wires running off the top of
the image are lead wires to connect to the IR LEDs. I have two sets of them
to double up the LEDs I connect to. You may notice I also added two
transistors, one for each LED connection. (Although they both share a
limiting resistor. I'm not sure why I made that design decision.) At the
right is a connector that fits directly over the pins on the Raspberry Pi.

{{< left >}}
{{< figure src="board-on-pi-thumbnail.jpg" width="310px"
link="board-on-pi.jpg" alt="board on raspberry pi" >}}
{{< /left >}}

At left is the same board installed on the Raspberry Pi. It is flipped
upside-down (so you can see my poor solder job) and the connector is
plugged into the I/O pins. Fortunately, the circuit is small enough that
with some careful placement of the components all the electronics can fit
within the standard Raspberry Pi case. The lead wires are ready to be
attached to connectors to the outside of the case.

{{< right >}}
{{< figure src="raspberry-pi-box-thumbnail.jpg" width="310px"
link="raspberry-pi-box.jpg" alt="circuit board" >}}
{{< /right >}}

Finally, at right is the Raspberry Pi fully in its case. On the top of the
case I've drilled two holes and installed 1/8" jacks to which the lead
wires are attached. These 1/8" jacks are the same type used for audio
headphones, but there are also several commercial IR blasters that use this
type of connection.

## The Raspberry Pi Configuration

Now that we have the hardware in order, we need to configure the Raspberry
Pi computer. Typically, the Raspberry Pi is run with some variant of Linux
as its operating system. There are several variants of Linux that run on
Raspberry Pi. When I got my Raspberry Pi I bought one that came with an SD
card pre-formatted with something called [NOOBS] that allowed me to install
a version of Linux called [Raspbian]. (If that sentence didn't make sense,
don't worry. The NOOBS setup is actually pretty self evident once you get
started.) If you don't have a pre-formatted memory card to start with,
[this page](http://www.raspberrypi.org/help/noobs-setup/) provides
information on how to get going.

Once the Raspberry Pi can boot Linux, it needs to be configured to send
signals to the IR hardware now connected to the hardware pins. We also have
some network configuration to make sure that the server will be
accessible.

[NOOBS]: http://www.raspberrypi.org/help/noobs-setup/
[Raspbian]: http://www.raspbian.org/

#### Linux Infrared Remote Control (LIRC)

Although the hardware described above is custom built, it is by no means
unique. Lots of folks have connected IR emitters to computers to send
signals. Because of this, there is a well established project named
[LIRC], which stands for Linux Infrared Remote
Control, that provides a mechanism to send IR remote commands from a
Linux-enabled device. This is good because it takes care of _a lot_ of
the software complexity.

LIRC requires special modules to be built in the Linux kernel, but, yet
more good news, the latest versions of Raspbian come with these modules
already built in. So all that is left is to install some system software
and modify some configuration files. I followed a setup outlined in [a blog
by Alex Bain]. I'll summarize (regurgitate? plagiarize?) the steps I took
here.

The first step is to use `apt-get` to install a package named lirc.

``` sh
sudo apt-get install lirc
```

Next, add the following to `/etc/modules` to install the LIRC kernel module
and configure it to output signals to pin 23 (where I connected the IR
circuit).

```
lirc_dev
lirc_rpi gpio_out_pin=23
```

Finally, the `/etc/lirc/hardware.conf` file needs to be modified for this
hardware configuration.

```
# /etc/lirc/hardware.conf
#
# Arguments which will be used when launching lircd
LIRCD_ARGS=""

#Don't start lircmd even if there seems to be a good config file
#START_LIRCMD=false

#Don't start irexec, even if a good config file seems to exist.
#START_IREXEC=false

#Try to load appropriate kernel modules
LOAD_MODULES=true

# Run "lircd --driver=help" for a list of supported drivers.
DRIVER="default"
# usually /dev/lirc0 is the correct setting for systems using udev 
DEVICE="/dev/lirc0"
MODULES="lirc_rpi"

# Default configuration files for your hardware if any
LIRCD_CONF=""
LIRCMD_CONF=""
```

(Note that this differs from [Alex Bain's configuration] in that I don't
include an argument to generate input events. Since the hardware is
output-only, such input configuration is unnecessary.)

> **Update** (3/19/2016): According to the most recent updates to [Alex
> Bain's instructions], you now also need to add the following line to
> `/boot/config.txt`.
>
> ```
> dtoverlay=lirc-rpi,gpio_out_pin=23
> ```
>
> The reason for this change is that the latest Raspberry Pi kernel has
> changed from implementing the lirc functionality as a kernel module to
> implementing lirc functionality through something called "Device Tree."
> The big win is that Device Tree allows you to use lirc without having to
> do a special recompilation of the Linux kernel (which is time consuming
> and quite difficult for us mere mortals). That wasn't such a huge deal
> for Raspberry Pi users since lots of binary distributions like [NOOBS]
> already compiled the kernel with lirc support, but it is still the right
> thing to do. More information is in [this Raspberry Pi forum article].
>
> I also think the addition to `/etc/modules` described above is unnecessary
> now, but I'm not sure.

Once LIRC is configured, the daemon needs to be restarted. It can be
restarted directly, but I find it easier (and more reliable but slower) to
just reboot the machine.

``` sh
sudo reboot
```

> **Update** (9/26/2017): Although the reboot command is a cure all, a
> faster method that works on most linux distributions now is to restart
> the daemon with the service command.
>
> ```sh
> sudo service lirc restart
> ```

[LIRC]: https://www.lirc.org/
[a blog by Alex Bain]: http://alexba.in/blog/2013/01/06/setting-up-lirc-on-the-raspberrypi/
[Alex Bain's configuration]: http://alexba.in/blog/2013/01/06/setting-up-lirc-on-the-raspberrypi/
[Alex Bain's instructions]: http://alexba.in/blog/2013/01/06/setting-up-lirc-on-the-raspberrypi/
[this Raspberry Pi forum article]: https://www.raspberrypi.org/forums/viewtopic.php?p=675658

#### Network Configuration

If I may rant a bit, I found interfacing with the network the most
frustrating part of this whole project. For a protocol that started over 40
years ago, it surprises me how difficult several things remain. Sure,
connecting the Raspberry Pi to the network is easy enough, by why is there
no standard facility for name lookup?

A name lookup converts the human assignable and readable name of a computer
to the numbers that specify the internet address. This is particularly
important when the numeric internet address can change. There is a simple
internet standard for performing name lookup named DNS. The basic way it
works is that you ask some set of known computers for the name lookup, and
they ask some other known computers until someone finds a match or no one
does. But if you have a local server hidden in a local network, the DNS
servers from your internet provider can never find it.

The simple solution is to have the router that provides internet to the
house to provide name lookup for local equipment connected to
it. Unfortunately, they don't. (Well, some do, but no router I've ever
owned.)

To get around this ridiculous oversight, multiple vendors have implemented
a solution called [multicast DNS]. Rather than rely on an external DNS
service, multicast DNS allows a service to broadcast its existence to the
local network. Although more complicated than a standard domain name
service, multicast DNS is simpler to set up and works quite well.

Sort of.

As I mentioned earlier, multiple vendors have implemented multicast DNS,
and these implementations are all incompatible with each other. No standard
has become the de facto implementation available everywhere. The most
widely accepted implementation is Apple's [Bonjour], which has since become
a [published standard]. This multicast DNS is built into any modern Apple
device, including their mobile devices. It is also freely available to
install for Windows (and is in fact part of the install for software like
iTunes, so many Windows systems already have it). There are also packages
for Linux (meaning it can be installed on the Raspberry Pi). Unfortunately,
I know of no way to install it on Android or Kindle devices, so I'm pretty
much SOL there.

Getting a Bonjour-compatible multicast DNS service on the Raspberry Pi is
simple. Just install the `libnss-mdns` package.

``` sh
sudo apt-get update
sudo apt-get install libnss-mdns
```
 
As soon as this module is installed, other Bonjour-compatible devices on
the same network will be able to see the Raspberry Pi as
`raspberrypi.local` (the multicast DNS adds the `.local` to the name to
ensure it does not conflict with real internet hostnames).

`raspberrypi` is not a great name for a TV remote service, so it is prudent
to change the hostname. I changed mine to `tvremote`. The hostname can be
changed by editing the `/etc/hosts` and `/etc/hostname` files and changing
the name `raspberrypi` to `tvremote` (or whatever you like). More details
are in [this
article](https://www.howtogeek.com/167195/how-to-change-your-raspberry-pi-or-other-linux-devices-hostname/).

As I complained about earlier, a major problem with multicast DNS is that
you cannot expect it to work with all devices. For those devices it does
not work with you simply have to type in the numbers for the IP address.
This is problematic if the Raspberry Pi is set up to configure its network
with DHCP, which can cause the IP address to change and is the default
configuration. So as a fallback, I gave my Raspberry Pi a static IP address
that is easy to remember. I'm not going into the details on setting that
up, but there are [instructions
available](https://www.modmypi.com/blog/tutorial-how-to-give-your-raspberry-pi-a-static-ip-address).

One word of warning about making changes to the network configuration: I
found that changing the network configuration could cause the multicast DNS
to behave oddly. I think it sometimes fails to pick up the network
configuration changes. There are probably more clever ways to update the
service, but I find it easiest to just uninstall and reinstall the module.

``` sh
sudo apt-get remove libnss-mdns
sudo apt-get install libnss-mdns
```

> **Update** (1/26/2016): The libnss-mdns module was working great for me
> for about a year and then it suddenly stopped finding other hosts. It
> looks like this functionality has been removed for security reasons. The
> functionality has been replaced with software named [Avahi] and can be
> installed with:
>
> ```sh
> sudo apt-get install avahi-daemon
> ```
>
> [More detailed instructions are available here.](http://www.howtogeek.com/167190/how-and-why-to-assign-the-.local-domain-to-your-raspberry-pi/)
>
> Installing the `avahi-daemon` package is sufficient for making the
> raspberry pi's hostname available to other devices, but will not make
> other devices' hostnames available to the raspberry pi. For that piece
> of functionality, you have to edit the file `/etc/nsswitch.conf` and
> change the line
>
> ```
> hosts: files dns myhostname
> ```
>
> to
>
> ```
> hosts: files mdns_minimal [NOTFOUND=return] dns myhostname
> ```
>
> I found this little solution on [the archlinux wiki].
>
> Did I mention networks are frustrating?

[multicast DNS]: https://en.wikipedia.org/wiki/Multicast_DNS
[Bonjour]: https://en.wikipedia.org/wiki/Bonjour_(software)
[published standard]: http://tools.ietf.org/html/rfc6762
[Avahi]: https://www.avahi.org/
[the archlinux wiki]: https://wiki.archlinux.org/index.php/Avahi#Hostname_resolution

## The Software

With the hardware and system ready to go, we need software to accept human
input and send the IR commands. As with any software project, there are
many valid approaches. I already talked about setting up LIRC to run the IR
hardware. I chose to write the service software in [Python], which is easy
to prototype in and has many modules readily available. I chose to
implement the software and network interface for the mobile devices as
active web pages. There are disadvantages to this approach, but it does
mean I can write one program that runs on all devices and easily distribute
the software.

[Python]: https://www.python.org

#### Using LIRC

The LIRC Linux module is configured with the file
`/etc/lirc/lircd.conf`. This file contains a bunch of codes that can be
sent from the IR emitter, each associated with a name and grouped by
devices, which also have names. OK, so where do those magic codes come
from?

Well, if you are feeling really industrious, you could build or buy a
circuit for an IR sensor and then use LIRC to record codes from your
existing remotes. I, however, am way to lazy for that. Fortunately, there
are a great many folks out there who are not as lazy as me, have recorded
signals from their remotes, and posted them to the [LIRC] site. They have a
[collection of supported remote controls] as well as [an open remote
database].

As helpful as these files are, there is a fundamental limitation. Because
these codes are recorded off of remotes, the commands are limited to those
found on the remotes. However, there are commands that are helpful to us
that are not found on a standard remote. For example, all TV remotes have a
power button that toggles the power (turns it off when it is on, on when it
is off). However, most modern TVs also have different signals that only
turn the TV on or turn the TV off. These signals are important if you want
to make sure that the TV is on or off and you cannot directly tell what
state it is currently in. Likewise, most TV remotes have a single button
that cycles through the inputs, but TVs often also support separate signals
to set a specific input.

So if these signals cannot be read from the remote, how do we find them?
Your best bet is to pull them from the internet from the manufacturer or
other sources. I found the [Remote Central] web site to have a good
collection.

Unfortunately, these manufacturer codes are seldom available in the
`lircd.conf` format needed by LIRC. Instead, they are commonly presented in
[ProntoEdit HEX format], which is a standard code for universal remote
codes. Fortunately, there is a tool for converting these pronto codes to
`lircd.conf` files named [pronto2lirc.py]. Although the codes
pronto2lirc.py generates will work with LIRC, the codes are rather ugly raw
codes. They can be reconfigured as the more standard readable codes by
running them through `irrecord -a` (which comes with an LIRC install). This
reads the codes as if they are coming from a sensor and process them the
same way to create the more standard codes. I used [this shell script] to
automate the conversion process.

Once all the necessary IR codes are collected and placed in
`/etc/lirc/lircd.conf`, we can use tools from LIRC to test sending
them. The LIRC module comes with a command named `irsend` that can be used
to start and stop the sending of IR commands. This is a good time to hold
an IR blaster next to the TV's sensor and try a few commands.

{{< right >}}
{{< figure src="ir-in-camera-thumbnail.jpg" width="310px"
link="ir-in-camera.jpg" alt="Picking up IR light in a digital camera." >}}
{{< /right >}}

Incidentally, it can be difficult to diagnose problems with the hardware if
things are not working because you cannot see the light from the IR
emitter. A trick you can use is to try to use a digital camera to pick up
the IR light. A lot (but not all) of digital cameras pick up light outside
the visible range, and the IR light will show right up in the display. The
picture at right shows just that. The bright spot on the remote is the IR
emitter. I cannot see it with the naked eye, but it shows up in the camera
display.

[collection of supported remote controls]: http://www.lirc.org/remotes/
[an open remote database]: http://lirc.sourceforge.net/remotes-table.html
[Remote Central]: https://www.remotecentral.com/
[ProntoEdit HEX format]: https://www.remotecentral.com/features/irdisp2.htm
[pronto2lirc.py]: https://github.com/aldebaran/lirc/blob/master/tools/pronto2lirc.py
[this shell script]: https://github.com/kmorel/EntertainmentCenterServer/blob/master/lirc_codes/pronto2hex

#### Helpful Python Modules

One of the nice things about using Python for projects like this is the
abundance of existing code available.

Several folks use Python to control LIRC and it is not hard to find code to
this effect. The [LIRC] site has a link to the pylirc module (now
superseded by [pylirc2]), but this module is really concerned with receiving
IR commands, which is not something we are particularly interested in.
Instead, I found a simple [module by Loisaida Sam] that does the trick. It
runs the `irsend` commands to generate the IR signals, and as an added
bonus it parses the `/etc/lirc/lircd.conf` file into a convenient
dictionary of codes.

The approach I took for my remote service was to have active web pages in
which the server sends commands through LIRC. One approach is to run a web
server like [Apache] or
[Lighttpd] and plug custom code into that. But a
simpler solution for development is to use a web service implemented in
Python that is designed to run Python functions on web
requests. [Flask] is a very good implementation of
such a service. There are other choices, but Flask is what I use for my
implementation.

[pylirc2]: https://pypi.python.org/pypi/pylirc2
[module by Loisaida Sam]: https://github.com/loisaidasam/lirc-python
[Apache]: https://httpd.apache.org/
[Lighttpd]: https://www.lighttpd.net/
[Flask]: http://flask.pocoo.org/

#### The Server Implementation

With the aforementioned Python modules as well as support from [jQuery] and
[jQuery Mobile] for the web page JavaScript, I implemented [this software
that can be accessed on github]. The code is geared to my equipment, but
should be straightforward to modify.

I'm not going to go into details on the software implementation. Chances
are you will either understand it or not. (OK, I'm just too lazy to
document it.) But here are a couple of screen shots taken from my phone.

{{< center >}}
{{< figure src="server-off-screen-thumbnail.png" link="server-off-screen.png"
alt="Server Screen Off." >}}
{{< /center >}}
&nbsp;

{{< center >}}
{{< figure src="server-br-screen-thumbnail.png" link="server-br-screen.png"
alt="Server Screen Blu-Ray." >}}
{{< /center >}}

[jQuery]: https://jquery.com/
[jQuery Mobile]: https://jquerymobile.com/
[this software that can be accessed on github]: https://github.com/kmorel/EntertainmentCenterServer

#### Managing the Server

One last detail is to make sure that the server is always running on the
Raspberry Pi device. When the computer first boots up, the server should
automatically be started. It is also a good idea to restart the service
occasionally.

The standard mechanism for services on Linux is to create init.d scripts.
Perhaps I will be flamed for saying so (assuming anyone actually reads this
blog), but I find this mechanism quite terrible. The script setup is
non-intuitive, it varies dramatically from system to system, and the
restart mechanism is very difficult to get working correctly.

I find a much better approach is to use a program called [Supervisor].
Supervisor is a meta-service that can start and stop other services. It is
much easier to set up, much more reliable in starting and stopping services
correctly, and as an added bonus has a nice web interface.

Supervisor can be installed on the system using the `easy_install` command
from the Python setuptools package.

``` sh
sudo easy_install supervisor
```

A configure file controls both the general Supervisor settings and the
programs Supervisor manages. By default this file is put in
`/etc/supervisor.conf`. Start the configuration by copying the sample
configuration.

``` sh
sudo cp /usr/local/lib/python2.7/dist-packages/supervisor/skel/sample.conf /etc/supervisord.conf
```

Supervisor's control tools work through sockets. By default, the Supervisor
configuration is set up using a file socket. I prefer opening a web socket
so that I can also configure remotely. To do this, comment the configure
file's section named `unix_http_server` and add (or uncomment) the
`inet_http_server` to have the following configuration.

```
[inet_http_server]
port=:9001
```

Also change the `serverurl` configuration (under the `supervisorctl`
section) to be an http connection.

```
serverurl=http://127.0.0.1:9001
```

And finally the configuration needs a `program` section for the TV remote
software. Here is the section I use.

```
[program:tvremote]
directory=/home/pi/src/EntertainmentCenterServer
command=/usr/bin/python ecserver.py
redirect_stderr=true
stdout_logfile=/tmp/tvremote.log
```

It is important for Supervisor to be launched as a Linux service when the
Raspberry Pi is launched. So, yes, we still need to set up an init.d server
script, but we need just one service to rule them all. The Supervisor
maintainers provide several example init.d scripts. The [script for Debian]
works well. Copy [this
file](https://github.com/Supervisor/initscripts/blob/master/debian-norrgard)
to `/etc/init.d/supervisor`. Edit this script to make sure `DAEMON` and
`SUPERVISORCTL` point to the correct places. I also added the arguments `-c
/etc/supervisord.conf` to `DAEMON_ARGS`. Once all this is in place, run

```
sudo update-rc.d supervisor defaults
```

to have Supervisor started on boot.

With Supervisor set up, the TV remote service can be controlled through a
remote web browser by connecting to http://tvremote.local:9001 (with
tvremote.local replaced with whatever the Raspberry Pi hostname is). The
service can also be restarted from the command line with the
`supervisorctl` command. I added the following to my crontab to restart the
service daily.

```
0 4 * * * /usr/local/bin/supervisorctl restart tvremote
```

[Supervisor]: http://supervisord.org
[script for Debian]: https://github.com/Supervisor/initscripts/blob/master/debian-norrgard

## Final Words

So with all this work done, how well does the system work? Well, it has its
benefits and drawbacks. I have to say that it is very nice being able to
control my entertainment center through my phone, if for no other reason
then I never have to look for the remote. It also gave me the opportunity
to design the interface specifically to my equipment configuration and my
own personal preferences.

By far the biggest problem I have is with unreliability in the network
connection between phone and server. Even when everything is working
perfectly, the server often takes several hundred milliseconds to respond.
A half a second might not seem like a long time, but it creates a lot of
frustration when trying to fast forward or rewind to a particular spot. So,
I sometimes end up reaching for a regular remote anyway. But at least I've
been losing remotes much less frequently.

As I finish writing this blog, my wife is reminding me how much time I
spent to save time when I'm wasting time. She may have a point, but
ultimately this is just a fun cool gadget. That's the real point.

Right?
