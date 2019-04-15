---
title: Latency & Throughput
date: 2019-04-11 00:00:00 Z
---

#### Basic Concept

- Latency is the time taken to accomplish a task, it is measured in unit of time.
- Throughput is number of tasks accomplished within a period of time, it is measured in whatever number of task has been accomplished within the unit of time.

Design example

- Let's assume clock frequency is 100 MHz (which means 100000000 oscillation/pulses or clock periods per second)
- Let's assume time taken for a computation task being 1000ns; which means 1.0E-6 seconds; **note** this indicates the latency of the task
- Let's assume throughput being 640 Mbits/second (which means 6.4e+8 bits/seconds)
- Let's assume word width of each output is 64 bit

Considering above facts below calculations shows that 1 operation will have a latency of 100 clock periods & there can 0.1 words at max produced by the system given a clock period; therefore 10 words can be produced within the latency period

```
Latency: 1000 ns = 1000 ns * (1 s / 10^9 ns ) * ( 100 * 10^6 clock periods/ 1s) = 10^11/10^9 = 100 clock periods.

Throughput = 640 Mbits / s = (640 * 10^6 bits/s) * (1 word / 64 bits) * ( 1 s / 100 * 10^6 clock periods) =  640 * 10^6 / 64 * 100 * 10^6 = 10 * 10 / 100 = 1 / 10 = 0.1 words / clock period.
```

##### References

1. https://community.cadence.com/cadence_blogs_8/b/sd/posts/understanding-latency-vs-throughput







