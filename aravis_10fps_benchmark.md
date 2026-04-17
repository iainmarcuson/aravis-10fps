Spectral Instruments 1600S 10 FPS Framerate
===========================================

Introduction
------------

It is required to receive data from a Spectral Instruments 1600S at 10 fps.  This requires GigE Vision transfer.  A modified version of Aravis was used to achieve this.

Hardware Setup
--------------

The camera setup consisted of the SI HTTP Server connected to a computer over a direct dedicated Gigabit Ethernet link.  The computer required a separate server-grade network card.  The setup looks like


                                                   |  Computer
    |Camera|------------|SI HTTP Server|-----------|Ethernet card     |
              Fibre                      Cat6      |On-board ethernet |------->
              Optic                                                     Network

The computer was a Dell OptiPlex 3046 running Windows 10 on an SSD.  The Ethernet card was an Intel PRO/1000 PT Server Adapter.  Aravis was running under msys2 in the ucrt64 environment.  Most of the programs on the computer were closed.

An external Ethernet card is likely required.  It must be server-grade.  Note there is no switch in the computer-HTTP Server connection.  The network consists of a direct connection with one Cat 6 cable.

Network Configuration
---------------------

It is essential that unnecessary services on the Ethernet connection be disabled.

Enable **Internet Protocol Version 4 (TCP/IPv4)**.  **Npcap Packet Driver** may stay enabled.  Disable **Client for Microsoft Networks**, **File and Printer Sharing for Microsoft Networks**, **QoS Packet Scheduler**, **Microsoft Network Adapter Multiplexor Protocol**, **Microsoft LLDP Protocol Driver**, **Internet Protocol Version 6 (TCP/IPv6)**, **Link-Layer Topology Discovery Responder**, and **Link-Layer Topology Discovery Mapper I/O Driver**.

Run Statistics
--------------

An image acquisition sequence was performed with `arv-camera-test`.  It is important to remember that parameters must be set with `arv-tool`.  For this run, `ExposureTime` was set to 9 ms and `FrameInterval` was set to 0.

`arv-camera-test` prints summary statistics at the end of its run.  The elapsed time may be measured by using the time command.  A transcript is below, with line wrapping and spaces added for clarity:

	$ time arv-camera-test-0.10.exe -n "Spectral Instruments, Inc.-1600-124" \
	     -w 2048 -h 1024 --features Binning=Bin4x4 FrameInterval=10000 \
         AcquisitionMode=Continuous -a -j on-failure -p 300 --realtime -i 8192 -m 600

	Looking for camera 'Spectral Instruments, Inc.-1600-124'
	vendor name            = Spectral Instruments, Inc.
	model name             = 1600
	device serial number   = 124
	image width            = 2048
	image height           = 1024
	exposure               = 9000 ┬╡s
	payload                = 3145728 bytes
	gv n_stream channels   = 1
	gv current channel     = 0
	gv packet delay        = 0 ns
	gv packet size         = 1500 bytes
	GC_EXEC about to check value.
	GC_EXEC passed value check.
	GC_EXEC passed write access check.
	GC_EXEC passed node get error check.
	n_completed_buffers    = 4365
	n_failures             = 0
	n_underruns            = 0
	n_timeouts             = 0
	n_aborted              = 0
	n_missing_frames       = 0
	n_size_mismatch_errors = 0
	n_received_packets     = 9389115
	n_missing_packets      = 0
	n_error_packets        = 0
	n_ignored_packets      = 0
	n_resend_requests      = 0
	n_resent_packets       = 0
	n_resend_ratio_reached = 0
	n_resend_disabled      = 0
	n_duplicated_packets   = 0
	n_transferred_bytes    = 13806516672
	n_ignored_bytes        = 0
	GC_EXEC about to check value.
	GC_EXEC passed value check.
	GC_EXEC passed write access check.
	GC_EXEC passed node get error check.
	
	real    5m7.774s
	user    0m0.015s
	sys     0m0.031s

An elapsed time of 5 minutes and 7.8 seconds gives a total time of 307.8 seconds.  The `n_completed_buffers` value of 4365 gives a frame rate of 14 fps.  The zero value for `n_failures` shows that all frames were transferred without error.  Note also that zero packets were missing, errored, ignored, resent, or duplicated.