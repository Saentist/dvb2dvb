dvb2dvb
=======

A tool to convert multiple VBR single-program DVB transport streams to
a CBR multi-program transport stream suitable for feeding to a DVB
modulator.


Introduction
============

dvb2dvb was designed to integrate tvheadend (http://tvheadend.org)
with a IT9507-based USB DVB-T modulator.  It allows you to read
multiple TV and radio channels from one or more tvheadend servers (via
http streaming) and to output those channels as a single DVB-T
multiplex.

Full metadata (channel names, channel numbers (LCNs) and EIT
(now/next)) are included in the output stream.

It has been tested with the Linux driver and "sendts" example
application available at

https://github.com/linuxstb/it9507/

The DVB-T modulator is available to purchase here:

http://www.idealez.com/hides/product-detail/en_US/69859

The input streams must contain as a minumum the PAT, PMT and SDT
tables.  These are rewritten by dvb2dvb, which also adds a new NIT.  EIT
(present/following) is rewritten and passed through to the output
stream if it is available in the input streams.

The input streams are multiplexed to a single output stream based on
the PCRs, which must be present in the input streams.  These are
sometimes contained within the video PID, but are often transmitted in
their own PIDs.

A recent (November 2014 or later) git master version of tvheadend
using the "passthrough" output format will generate suitable streams
to be used with dvb2dvb.

Encrypted streams are fully supported by dvb2dvb, assuming they can be
descrambled by tvheadend.  dvb2dvb will remove all CA-related
descriptors and flags from the streams, so they appear as FTA services
to the DVB-T receiver.


Building
========

Just type "make" in the source code directory.  Two libraries are
required - pthreads and libcurl.

ex.
sudo apt-get install libcurl4-openssl-dev


Current status
==============

dvb2dvb is still under development.  The basic functionality is
working but not all the stream output parameters are user-configurable
and the code is not stable or resilient to errors in the input
streams.


Usage
=====

./dvb2dvb config.json

See example.json for an example configuration file.  Note that dvb2dvb
currently only supports one output mux, although the config file
format allows for more. (This feature will be added in the near future).


Copyright/Licence
=================

dvb2dvb is (C) 2014 Dave Chapman.  

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License along
with this program; if not, write to the Free Software Foundation, Inc.,
51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.


dvb2dvb includes json-parser from https://github.com/udp/json-parser -
see the headers in json.[ch] for licensing information.
