---
title: "Different IO methods for Linux"
date: 2017-10-08
draft: false
tags: ["I/O", "MMAP", "Read/Write", "Direct IO", "Async Direct IO", "AIO", "DIO"]
---

# Different Access I/O methods for Linux

Types of Access:

1. Read/Write
2. MMAP
3. Direct I/O Read/Write (DIO)
4. Asynchronous Direct I/O Read/Write(ADIO)

## Read/Write:
Traditional Read/Write and its variants(`pread`, `preadv` etc...) use `read(2)` and `write(2)` for reading and writing. Kernel will copy the required read data in the process address space and returns. If the requested data is available in page cache, kernel will return the data from page cache immediately. If not, blocks the thread and requests the disk to fetch the requested data. Once the data is available, thread is resumed and the data is returned. For writes, it just simply puts the data into page cache and returns. The data is later pushed to disk. If the page cache is full, thread is blocked and write is done to disk and then resumed.

## MMAP:
In memory map, the application address space is mapped to the page cache. During the read, if cache hit, the data is taken directly without copying and bypassing kernel entirely. If cache miss, thread is blocked and data is requested from the disk. Once received, it maps the page cache to application address space and returns. For writes, its directly written to page cache and disk write is done later. If the page cache is full, blocked write to disk.

## Direct I/O (DIO):
Both the above methods defer the scheduling to kernel at a later point. If the application wishes to control the scheduling, it can use the `O_DIRECT` flag which will block the thread for read/write from/to disk. Here the cache is completely bypassed. Furthermore, disk controller will copy the contents to application space bypassing kernel.

## Asynchronous Direct I/O:
Similar to DIO but the thread will not be blocked. Once submitted to `io_submit`, another thread `io_getevents()` will wait on the operations to be completed and collects the data. Again, page cache is bypassed and disk controller copies the data directly into application space.

## Advantages and Disadvantages:

### Cache-Control:
For the Read/Write and MMAP, cache plays an important role in retrieving the data. The cache is fully managed by kernel and applications has limited say on how the data is saved to file or which page in the cache should be evicted if there is no space.
The advantage of kernel managing the cache is that, a lot of time, effort and research went into to optimizing those algorithms.
The disadvantage is that, these algorithms are general purpose and hence, application specific optimizations are not possible.

### Copying:
For the first method, kernel copies the data from page cache to application space and back. But with MMAP, kernel is completely bypassed and data is is directly copied to application space from cache page. This works well when the storage size and ram size ratio is close to 1:1.
But if the storage size and RAM ratio is way high i.e.. 100:1, once the page is cached, due to smaller ram size, another page must be evicted from the cache if there is no space. Also, MMAP requires 0.2% size of storage size for page tables. So, when the page is evicted from the cache, kernel has to re-scan the page tables for inactive pages. If the storage size is higher, so does the Page table. Ex: 100:1 leads to 20% of the storage size.

### I/O scheduling:
With Read/Write and MMAP, kernel is responsible for I/O operations and the application has no control over it. This leads to problems like:
1. When kernel decides to do larger writes, all the read I/O might get blocked.
2. Kernel cannot distinguish between important and un-important tasks leading to background tasks overwhelming and there by increased latencies for foreground tasks.

With DIO and AIO, this can solved since the complexity can be managed by the applications since they have complete control over I/O scheduling.

### Thread Scheduling:
With Write/Read or MMAP, since the I/O scheduling is handled by kernel they will not know what the cache hit rate is and hence, they might start a lot of threads for read and write. This can be inefficient for machines with lesser cores and if there are a lot of cache misses.

With Direct I/O, the application will know when the thread is blocked and limit the threads and with Async Direct I/O, the application will know everything and can limit the threads efficiently based on waiting and available threads.

### I/O alignment:
Kernel usually takes care of assigning correct block size to perform I/O. If kernel is bypassed, the application must ensure the correct block size is chosen.

### Application Complexity:
In the case first two, application complexity will much lesser than the latter two due to custom I/O scheduling algorithms.

*More info can be found [here](http://www.scylladb.com/2017/10/05/io-access-methods-scylla/)*
