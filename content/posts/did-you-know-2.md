---
title: "Did You Know: Chapter 2"
date: 2017-09-27
tags: ["tld", "cmd", "linux", "tgz"]
draft: false
---

1. In the Soviet era Russia your land could be transferred to others by the state and hence was not private property.
2. The .COM TLD was one of the first created in 1985
3. Root name servers
    1. There are 13 root name server around the world operated by 12 independent organisations
    2. Each name server starts with [name].root-servers.net, with name replaced with any of chars including [a-m]
    3. Even though there are 13 root name servers, each of the 13 organisations that maintain these servers deploys multiple physical servers around the globe
    4. This basically means, there is load balancer for each root name server before all the physical servers deployed in different locations
    5. Infrastructure TLDs: .ARPA, mostly used for reverse DNS lookups.
4. International charges to credit-card accounts are cleared per country basis. And charges made in different countries are reconciled periodically. Thus bank balance can be different in different countries
5. DNA remains in the nucleus, while RNA can enter the far reaches of the cell to carry out DNA's instructions.
6. `readlink -f file.txt` will fetch the full path of `file.txt`
7. List contents of tar gz and fetch only one file
    1. Tar tf file.tgz
    2. Tar-xf file.tgz filename
8. Nice trace route - `mtr google.com`
9. Print text ad infinitum
    1. `yes` prints yes infinitely
    2. `yes hello` prints hello infinitely
10. `w` prints who is logged in
11. `ls | nl` prepends line number
12. `tac file` backwards of `cat` prints from end to start
13. Run a command everytime file is modified
    1. `while inotifywait -e close_write document.tex;do;make;done`
14. Keep program running after leaving ssh
    1. Nohup ./program &
