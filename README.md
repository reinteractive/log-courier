# Log Courier [![Build Status](https://travis-ci.org/driskell/log-courier.svg?branch=stable)](https://travis-ci.org/driskell/log-courier)

Log Courier is a tool created to transmit log files speedily and securely to
remote [Logstash](http://logstash.net) instances for processing whilst using
small amounts of local resources. The project is an enhanced fork of
[Logstash Forwarder](https://github.com/elasticsearch/logstash-forwarder) 0.3.1
with many enhancements and behavioural improvements.

## Features

Log Courier implements the following features:

* Tail log files, following rotations and resuming at the last offset on
restart
* Read from standard input for lightweight shipping of a program's output
* Extra event fields, arrays and hashes on a per file basis
* Fast and secure transmission of logs using TLS with both server and client
certificate verification
* Secure transmission of logs via CurveZMQ to multiple receivers simultaneously
(optional, requires ZeroMQ 4+)
* Plaintext transmission over plain ZMQ and TCP when security is not required
* Multiline codec to combine multiple lines into single events prior to shipping
* Load multiple configuration files from a directory for ease of use with
configuration management
* Reload the configuration without restarting

Log Courier integrates with Logstash using an event receiver ruby gem. An event
sender ruby gem is also available to allow fast and secure transmission between
two Logstash instances.

## Installation

### Build Requirements

1. The [go](http://golang.org/doc/install) compiler tools (>= 1.1.0)
1. [Logstash](http://logstash.net) 1.4.x
1. (Optional) [ZeroMQ](http://zeromq.org/intro:get-the-software) (>= 4.0.0)

### Building

To build with the optional ZMQ support use the following.

	git clone https://github.com/driskell/log-courier
	cd log-courier
	make with=zmq

Otherwise, simply run make standalone as follows.

	git clone https://github.com/driskell/log-courier
	cd log-courier
	make

The log-courier program can then be found in the 'bin' folder.

*If you receive errors, try using `gmake` instead.*

### Logstash Integration

Details instructions on how to integrate with Logstash can be found on the
[Logstash Integration](docs/LogstashIntegration.md) page.

### Generating Certificates and Keys

After Log Courier is built you will find a utility named lc-tlscert inside the
'bin' folder alongside the main log-courier program. This will generate a
self-signed certificate to get you started quickly with the TLS transport, and
the necessary Log Courier and Logstash configuration snippets to make it work.

Likewise, a utility called lc-curvekey is produced when ZMQ support is enabled.
This utility will generate CurveZMQ key pairs as well as the necessary
configuration snippets.

## Command Line Options

The log-courier command accepts the following command line options.

	-config="": The config file to load
	-config-test=false: Test the configuration specified by -config and exit
	-cpuprofile="": write cpu profile to file
	-from-beginning=false: Read new files from the beginning, instead of the end
	-idle-flush-time=5s: Maximum time to wait for a full spool before flushing anyway
	-list-supported=false: List supported transports and codecs
	-log-to-syslog=false: Log to syslog instead of stdout
	-spool-size=1024: Maximum number of events to spool before a flush is forced.
	-version=false: show version information

## Documentation

* [Configuration](docs/Configuration.md)
* [Change Log](docs/ChangeLog.md)
