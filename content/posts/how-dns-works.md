---
title: "How DNS Works"
date: 2017-09-20
tags: ["dns", "root-name-server", "TLDs", "name-servers"]
draft: false
---
*Note: If you find any issues in the post, let me know in the comments section*

This is a short post how DNS works. Prefer more comical learning, then go here - https://howdns.works/

So what happens when you type in a website address?
User entered address https://vedhavyas.com. Browser needs to resolve this address to an public IP address.

1. Browser cache
    1. Browser check its local cache for an IP address that maps to given website address
    2. If found, call the IP address to serve the request. Job done.
    3. If not found? Move to next step
2. OS cache
    1. Browser asks os to resolve an IP from the address
    2. OS checks its cache. If found, let browser know about it
    3. Browser will save the ip to its cache and request is served
    4. If not found? Move to resolver
3. Resolver cache
    1. OS will then ask the resolver for resolve the Address
    2. Resolver is your internet service provider resolver. This is how your ISP can track your internet activity. ISP such as ACT may even block the site by not resolving the address.
    3. So, how can you unblock it? You can use a different resolver instead of your ISP resolver. I prefer DNSCrypt. It provides a wide range of DNS resolvers to pick from.
    4. Now back to resolving request. Resolver will check its cache for the IP address. If found, return back to browser and request is served.
    5. If not found? Move to root name servers
4. Root Name servers cache
    1. Every resolver should know at least one root name servers
    2. There are 13 root name servers around the world.
    3. Now root name server will search its cache for IP. If found, give it resolver
    4. If not found, it give resolvers the address of the Top Level Domain of the address to talk to. In our case its “.com” TLD, one of the largest TLD's
5. TLD cache
    1. If TLD has IP info of the address, gives it back to resolver
    2. If not, TLD will provide address of the name servers of the domain “Vedhavyas.com”
    3. In our case, TLD will provide the following name server address
        1. alex.ns.cloudflare.com
        2. cass.ns.cloudflare.com
    4. But how would TLD would know the name server address?
        1. Domain registrar. Whenever you buy a domain from a domain registrar, They communicate the name server details with the TLD
        2. My Domain registrar here is namecheap.com. This is where I provide them the custom name servers of cloudflare.com
        3. Else, namecheap.com will provide their own name servers
    5. Whats happens if the name server is a subdomain of the website we have asked for? Wouldn’t that be an infinite loop?
        1. Well, TLD provides name server address along with their IPs.
        2. So that resolver can directly talk to name server since those addresses are already resolved
    6. Why multiple name servers?
        1. Just in case one goes down :p
6. Name Servers
    1. Name servers should know the IP address of the website, given website owner provided necessary details
    2. If not preset, then you will see a sad smiley on your browser saying failed to resolve the address.
    3. In our case, I have provided cloudflare.com required IP address of my website. Sometimes there can be multiple IP addresses as well. Just to in case ;p
    4. My IP address are “192.30.252.153”, “192.30.252.154”. These are IP addresses of GitHub.com server where my website is hosted
7. Return
    1. Now resolver will cache all the necessary info during this trip to about any further trips
    2. The IP address is given to your browser and the website is served.
