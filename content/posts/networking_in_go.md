---
title: "Networking in Go"
date: 2017-10-08
draft: false
tags: ["tcp", "udp", "ip", "networking", "OSI"]
---
* Architecture
    * Network protocols
        * ISO OSI - Open systems interconnect
            * Application
            * Presentation
            * Session
            * Transport
            * Network
            * Data Link
            * Physical
        * TCP/IP - Transmission control/ Internet Protocol
            * Application (Application, presentation, session)
            * TCP / UDP (transport)
            * IP (Network)
            * H/w interface (Data Link, Physical)
        * Other protocols include
            * Bluetooth
            * WiFi
            * USB
            * Firewire
    * Gateways
        * Repeater operates at physical layer and copies data from one subnet to another
        * A bridge operates at Data Link layer and copies frames between networks
        * Router operates at transport layer and not only moves data between networks but also decides on route
    * Flow
        * Each layer add a header to the packet from top to bottom
        * Once received, each header is removed at the specific layer
    * Connection Models
        * Connection oriented
            * Single connection is used to transfer packets from one network to other
            * Once done, connection is closed
            * Ex: Telephone conversation
            * TCP
        * Connectionless
            * Each transfer is sent independently
            * Ex: ordinary mail
            * IP
            * Connection networks are built over connection less. Ex: TCP over IP
            * Connection less are built over connection. Ex: HTTP over TCP
    * Distributed Computing Models
        * Client - Server
            * Client sends a request and server replies back
        * Peer - peer
            * Two components talk to each other in bi-directional way
            * I.e.. both can request and both can respond
        * filter
            * Data is sent to a middle component which applies filter to the data and is passed on to third component
            * Ex: Model -> Controller -> View
    * Gartner’s classification of Component Distribution
        * Distributed Data
            * Small portion of data is held locally and large chunk remotely
            * Ex: Mobile phone
        * Remote Data
            * Data is completely held remotely
            * Ex: Network file service
        * Distributed Transaction
            * Logic is shared locally and remotely
            * Ex: Web
            * HTML javascript locally, server remotely
        * Remote Presentation
            * Ex: SSH
        * Distributed Presentation
            * Ex: GUI
* Socket-level programming
    * TCP is connection oriented protocol
    * UDP (User Datagram Protocol) is a connectionless protocol
    * IP Datagram
        * It is a connection less, unreliable delivery system
        * Every datagram is considered independent
        * It include checksum of source and destination to the header
        * It also takes care of dividing and assembling larger datagrams to smaller and vice versa
    * UDP Datagram
        * It is a connection less protocol.
        * It adds to IP, checksum of contents and ports
    * TCP Datagram
        * It provides reliable delivery system
        * Services are identified by port numbers
* Data Serialisation
    * Data is sent across networks as sequence of bytes
    * TCP and UDP provide mechanisms for data transfer
    * Application has to take care of marshalling and unmarshalling data. Typically converting the data structures to stream of bytes and back to data structures on the receiving end
    * The Unmarshalling has to how the data was serialised in order to unmarshall it correctly
    * XDR by sun
        * External Data representation
        * An early serialisation technique
        * It type unsafe since no type info was provided in byte stream for unmarshaller
        * Correctness was ensured by generating marshaller and unmarshaller by the compilers itself
    * Self describing data
    * ASN.1
        * Go does not support full list of ASN types and structures
    * JSON
    * GOB
        * Serialisation technique specific for Go types and structures
        * Doesn’t support by anyone out there
    * Go also supports binary to text formats
        * Base64 is one such
* Data Transfer
    * HTTP transfer types:
        * Byte encoded
            * First part of the message is single byte to distinguish between different encodings
        * Character encoded
            * Message in the first line is a character to distinguish
