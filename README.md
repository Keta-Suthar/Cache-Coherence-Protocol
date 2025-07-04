# Cache-Coherence-Protocol
Implementation of cache coherence protocol 
This project implements a **multi-core cache coherence simulator** for shared-memory systems. It models private L1 caches for each processor and simulates two widely studied cache coherence protocols: **MESI** and **MOESI**. The simulator allows analysis of cache behavior under different coherence models and memory access traces.

---

## Project Overview

- Purpose: Simulate and analyze cache coherence protocols
- Architecture: Each processor owns a private L1 cache
- Protocols: Supports MESI (4-state) and MOESI (5-state) coherence mechanisms
- Flexibility: Can simulate infinite or finite-size caches
- Tracking: Monitors performance metrics, memory transactions, and protocol-specific behavior

# Working of Project:
## Processor and Cache Model
- Each processor is simulated with its own L1 data cache
- Caches are:
  - Fully associative if --cache-size = -1
  - Set-associative otherwise (based on --assoc)
- Memory accesses are driven by a trace file specifying:
  - Data stored in trace file is of format: <processor_id> <r/w> <hex_address> for example: 1 w 0x00000010
  
## Cache Coherence Protocols
- MESI protocol:
  - States: Modified, Exclusive, Share, Invalid
- MOESI protocol:
  - States: Modified, Exclusive, Share, Invalid, Owned

## Bus Transactions:
- All caches snoop bus operations:
  - BusRd: A processor read miss
  - BusRdX: A processor write miss
  - BusUpgr: A processor write hit on a shared line
- These trigger actions like:
  - Invalidations in peer caches
  - State transitions (e.g., Shared → Invalid)
  - Flushes if dirty lines exist

## Replacement Policy
- Implements LRU (Least Recently Used) evictio
- On cache fill:
  - Invalid lines are reused first
  - If none, the least recently used line is evicted
- If the evicted line is in Modified state, a flush/writeback is performed

## Statistics Collected
Read/write counts
Miss and hit rates
Invalidations and flushes
Memory transactions
Total execution time (based on latency model)

## Latency Model
Operation --> Latency (cycles)
- Read Hit --> 1
-  Write Hit --> 3
- Memory Access --> 100
- Flush/Writeback	--> 15

# Instructions used to debug and build file:
- make clean
- make debug

# Project simulation commands:
- make test_mesi
- make test_moesi

# Summary
This simulator provides a practical way to:
- Analyze cache behavior in multiprocessor systems
- Compare MESI vs MOESI protocol performance
- Observe how coherence protocols handle shared memory traffic
The design is modular, extensible, and understand the nuances of cache coherence.

Output: can be found in [output folder](https://github.com/Keta-Suthar/Cache-Coherence-Protocol/tree/main/Output).
For more information feel free to contact me at ksuthar@ncsu.edu 
