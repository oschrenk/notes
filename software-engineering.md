# Software Engineering #

## Talks ##

[Greg Wilson](http://en.wikipedia.org/wiki/Gregory_V._Wilson) gave a terrific talk called [What We Actually Know About Software Development, and Why We Believe It's True](http://vimeo.com/9270320) during the Cusec conference on 2010-Feb-07.

It approaches software engineering topics like programming paradigms with more evidence based methods.

### Quotes ###

> Most people would rather fail than change.
> Engineering is what happens when you put science and economics together

## Dimensions & Sizes ##

### Computing units ###

- `b` = bit.
- `B` = byte or 8 bits or 8b
- `g` = gram
- `kb` = kilobit or 1000 bits. kilo being the scientific prefix for 1000 like - `k`ilometer.
- `kB` = kilobyte or 1000 bytes, sometimes used for storage
- `kg` = kilogram
- `Kb` = Kibit = kibibit or 1024 bytes.
- `KB` = KiB = kibibyte or 1024 bytes, sometimes used for memory
- `Mb` = megabit = 1000^2 bits or 125 kB. Mb/s is used for network bandwidth.
- `MB` = megabyte = 1000^2 bytes, MB/s is used for disk bandwidth.
- `MiB = mebibyte = 1024^2 bytes, often referred to as MB or Megabyte for memory.
- `mb` = millibits = 0.001 bits - never use correctly.
- `mB` = millibytes = 0.008 bits - could be used for compression, but isn't
- `Gb` = gigabit = 1000^3 bits or 125 MB, Gb/s is used for network bandwidth.
- `GB` = gigabyte = 1000^3 bytes, used for disk space, GB/s is used for memory-bandwidth
- `GiB` = gibibyte = 1024^3 bytes, often referred to as GB or Gigabyte for memory.
- `gb` = gram-bit (also means to feel itchy)
- `gB` = gram-byte (someone's Xbox id)
- `TB` = Terabyte = 1000^4 bytes, used for disk space.
- `TiB` = Tebibyte = 1024^4 bytes, used for memory but usually written as TB.
- `PB` = Petabyte = 1000^5 bytes, used for large storage solutions.

[Computing Units Don't Have to Be Confusing](http://vanillajava.blogspot.nl/2013/11/computing-units-dont-have-to-be.html)

### Latency ###

    L1 cache reference ......................... 0.5 ns
    Branch mispredict ............................ 5 ns
    L2 cache reference ........................... 7 ns
    Mutex lock/unlock ........................... 25 ns
    Main memory reference ...................... 100 ns
    Compress 1K bytes with Zippy ............. 3,000 ns = 3 µs
    Send 2K bytes over 1 Gbps network ....... 20,000 ns = 20 µs
    SSD random read ........................ 150,000 ns = 150 µs
    Read 1 MB sequentially from memory ..... 250,000 ns = 250 µs
    Round trip within same datacenter ...... 500,000 ns = 0.5 ms
    Read 1 MB sequentially from SSD* ..... 1,000,000 ns = 1 ms
    Disk seek ........................... 10,000,000 ns = 10 ms
    Read 1 MB sequentially from disk .... 20,000,000 ns = 20 ms
    Send packet CA->Netherlands->CA .... 150,000,000 ns = 150 ms

[Latency numbers every programmer should know](http://architects.dzone.com/articles/every-programmer-should-know)


## Branching Strategies & Versioning Control ##

[Branching strategy is not a remedy for instability](http://altdevblogaday.com/2012/02/09/branching-strategy-is-not-a-remedy-for-instability/)

> Branching strategy changes are in response to the instability that follows fast growth.
> Branching is not designed to fix code instability. Branching is a way to isolate changes and manage a release.

Branching isn't a solution, you need a cultural shift towards testing and better infrastructure. These shift takes time and effort.