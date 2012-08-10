iu-splunk-cisco-ta
==================

Ian's Splunk Cisco Technology Add-On

--[ Introduction ]--

This technology add-on sets sourcetype information for incoming Cisco logs in the syslog format.  This transform classifies generic Cisco events.

For general Cisco events, it is assumed that the Administrator has the ability to configure a separate network input for general Cisco devices.

This is inspired by the Cisco Splunk apps, where I wanted something lightweight to classify traffic on a heavy forwarder, but wasn't doing any indexing.