iu-splunk-cisco-ta
==================

Ian's Splunk Cisco Technology Add-On for Splunk Input

--[ Introduction ]--

This technology add-on sets sourcetype information for incoming Cisco logs in the syslog format.  This transform classifies generic Cisco events.  This app should be deployed on Splunk instances that take input from Cisco devices (e.g. indexers, full stacks, heavy forwarders).

This is inspired by the series of Cisco Splunk apps, where I wanted something lightweight to classify traffic on a heavy forwarder, but wasn't doing any indexing.

I have tried to maintain as much compatibility with the original inputs as possible.  This has been verified to be compatible with:

* Splunk_CiscoFirewalls

--[ Installation ]--

For general Cisco events, it is assumed that the Administrator has the ability to configure a separate network input for general Cisco device, as well as configuring the Cisco devices to point to the newly defined input.

My personal recommendation is to configure separate TCP and UDP ports which with a generic source type of "cisco".  This can be done in the GUI or within your inputs.conf file in a manner similar to below:

[udp://5170]
connection_host = dns
sourcetype = cisco
no_priority_stripping = true
no_appending_timestamp = true

The last two lines are optional, but will retain the Syslog priority level and honor the timestamp as it arrives from your devices.  This assumes that all your Cisco devices have access to an NTP time source of some kind.  A recommended best practice is to send all logs to Splunk sourced in UTC which will ensure accurate logging in areas that are subject to varying rules regarding daylight time changes.

TCP syslog does not discard the priority:

[tcp://5170]
connection_host = dns
sourcetype = cisco

If you are configuring a TCP source, there have been cases where events close together will not be split into separate events.  This can be accomplished with the following in props.conf:

[source::tcp:5170]
LINE_BREAKER = ([^\>])\<\d{1,3}\>

Please test and validate Cisco devices which are using TCP logging on a separate system.  There are a number of known issues with Cisco devices not restarting their TCP logging if the connection gets interrupted for any reason.

IOS routers running IOS 12.4 or higher appear to work well with TCP and have no difficulty recovering.