---
title: "Did You Know: Chapter 1"
date: 2017-09-18
tags: ["network", "circuit-switching", "packet-switching", "circuit-switching vs packet-switching", "buffer-bloat", "Go", "Golang", "AIMD"]
draft: false
---
* The first internetwork message sent was “lo”, first 2 letters in “login”, since the receiving system crashed after “lo”.
* Circuit Switching Vs Packet Switching
    * Circuit Switching
        * The data is transferred in 3 phases
            * Connection established
            * Data Transfer
            * Connection Released
        * Each data unit knows the entire path since the path the connection is explicitly established for this transfer
        * Hence, this is very reliable
        * The data unit is processed at the Receiving end
        * The delay between data unit is uniform
        * But wastage of lots of resources
    * Packet Switching
        * Data is transferred directly
        * Packet only know the end destination but the intermediate nodes may change
        * Its not very reliable
        * Data is processed at intermediate nodes as well as destination node
        * The delay between data units is not uniform
        * But very less resource wastage
* In packet switching, due to less reliability of the transfer, we end up doing acknowledgements to and fro. But voice providers like Skype, while using packet switching, doesn’t use or use less acknowledgements since we humans provide robust support for packet loss. If there is a packet loss, we end up with clutter voices and we start to ask the other person to repeat again. There by resending the loss transmission again.
* In Go, when ever you are marshaling the byte slice, it will do a base64 encoding to avoid loss. Unmarshaling works from base64 decoding to byte slice
* I always assumed that in Go, for the value receiving methods, go does the dereferencing from pointer to value when called the function on pointer of that type and other way round didn’t work where calling pointer receiving method from type value instead of pointer. Seems like Go does the conversion of value to pointer as well and changes made to type data persist as well
* What is buffer bloat?
    * Buffer bloat in modems is where modems takes in packets and puts them in queue even though the queue is super long.
    * So when you start the upload a file, and as the queue builds up, the consequent request are added to queue and there by slowing down responses
* How is AIMD used in packet transfers?
    * AIMD - Additive increase and exponential backoff is used in packet switching when two or more connections compete to transfer data at the same instant form a single network
    * AIMD won’t kicks in until the first packet is dropped and then does drops the transfer rate by 50%
    * Once the next packet go through, increase the packet rate by one until next packet drop
