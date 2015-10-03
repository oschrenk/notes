# Time Scale of System Latencies

| Event                                     | Latency  | Scaled            |
| :---------------------------------------- | :------- | :---------------- |
| 1 CPU cycle                               |   0.3 ns |       1 s         |
| Level 1 cache access                      |   0.9 ns |       3 s         |
| Level 2 cache access                      |   2.8 ns |       9 s         |
| Level 3 cache access                      |  12.9 ns |      43 s         |
| Main memory access (DRAM, from CPU)       |   120 ns |       6 min       |
| Solid-state disk I/O (flash memory)       |  50-1 µs |     2-6 days      |
| Rotational disk I/O                       |  1-10 ms |    1-12 months    |
| Internet: San Francisco to New York       |    40 ms |       4 years     |
| Internet: San Fransisco to United Kingdom |    81 ms |       8 years     |
| Internet: San Francisco to Australia      |   183 ms |      19 years     |
| TCP packet retransmit                     |   1-3 s  | 105-317 years     |
| OS virtualization system reboot           |     4 s  |     423 years     |
| SCSI command time-out                     |    30 s  |       3 millennia |
| Hardware virtualization system reboot     |    40 s  |       4 millennia |
| Physical system reboot                    |     5 m  |      32 millennia |

