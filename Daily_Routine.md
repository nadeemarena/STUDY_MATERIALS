

Final HFT C++ project roadmap
Phase 1 — Market microstructure + order book

~15 programs

Basic limit-order matching-22nd Sept

Market-order execution
Partial fills
Multi-level market-order sweep
IOC
FOK
Cancel / replace
Order lifecycle state machine
Price-time priority
Pro-rata matching
L1 order book
L2 order book
L3 order book
Order-book replay
Snapshot + incremental book reconstruction

Learn: spread, liquidity, queue position, slippage, matching, order lifecycle.

Phase 2 — C++ data structures + memory

~12 programs

std::map order book
unordered_map order book
Flat-array order book
Preallocated order objects
Object pool
Memory pool
std::pmr
Custom allocator
AoS vs SoA
Cache-line alignment
Allocation benchmark
Object-lifetime benchmark

This is where I'd keep the allocator work because it gives you excellent interview material.

Phase 3 — CPU architecture

~10 experiments

Sequential vs random memory access
Cache-local vs cache-unfriendly order book
False-sharing demonstration
Branch-prediction benchmark
Branchless matching experiment
Cache-line alignment experiment
Prefetch experiment
TLB/page-size experiment
SIMD experiment
CPU-cycle benchmark

Measure with:

perf stat
perf record
perf report

Don't just learn CPU architecture theoretically. Make the CPU behavior visible.

Phase 4 — Concurrency

~12 programs

Mutex order book
Spinlock order book
Producer/consumer queue
SPSC ring buffer
MPSC queue
MPMC queue
Lock-free ring buffer
CAS-based queue
Acquire/release experiment
Relaxed-memory-order experiment
False-sharing experiment
Lock-contention benchmark

This should be a major phase for you because it directly connects C++ knowledge with low latency.

Phase 5 — Linux / OS

~12 programs

mmap shared-memory feed
epoll event loop
io_uring event loop
clock_gettime benchmark
rdtsc timestamping
CPU affinity
Thread pinning
Context-switch measurement
Page-fault experiment
Huge-page experiment
NUMA experiment
syscall-cost experiment

Tools:

perf
strace
ftrace

This connects very nicely with the Linux/performance preparation you've already been doing.

Phase 6 — Networking

~12 programs

TCP order gateway
UDP market-data feed
Binary protocol decoder
UDP multicast
Sequence-number handling
Packet-gap detection
Feed recovery
Snapshot recovery
Zero-copy parser
Message batching
Network latency measurement
Hardware timestamping / PTP concepts

I would add PTP/time synchronization here. It was missing from our earlier list and is worth understanding for distributed low-latency systems.

Phase 7 — Complete tick-to-trade system

Now combine everything:

Market Data
     ↓
Feed Handler
     ↓
Decoder
     ↓
Sequencer
     ↓
Order Book
     ↓
Strategy
     ↓
Risk
     ↓
Order Manager
     ↓
Serializer
     ↓
Order Gateway
     ↓
Exchange

Build:

Feed handler
Sequencer
Strategy interface
Risk engine
Order manager
Order gateway
Execution-report handler

This is the point where your individual experiments become one actual HFT system.

Phase 8 — Trading strategies

I'd actually squeeze this section down.

You don't need 20 sophisticated strategies for an HFT C++ engineering interview.

Build:

Market making
Order-book imbalance
Microprice
Simple momentum
Simple mean reversion
Cross-venue arbitrage

That's enough to understand how the strategy interacts with the infrastructure.

Phase 9 — Execution algorithms
TWAP
VWAP
POV
Liquidity-seeking execution
Smart order routing
Multi-venue order splitting

Again, don't over-expand this. You're targeting HFT engineering, not quant research.

Phase 10 — Risk + production behavior

This is where I'd extend the roadmap.

Position limits
Notional limits
Order-rate limits
Price-band checks
Duplicate-order prevention
Self-trade prevention
Kill switch
Stale-market-data detection
Exchange disconnect
Order reconciliation
Feed recovery
Process restart/recovery
Phase 11 — Failure engineering

This deserves its own section because it's extremely good system-design interview material.

Packet loss
Out-of-order messages
Duplicate messages
Feed gap
Exchange disconnect
Order acknowledgement timeout
Cancel rejection
Replace rejection
Strategy crash
Network failure
Exchange failover

The key question becomes:

What happens if something fails at every point in the tick-to-trade path?

Phase 12 — Latency engineering

This is the final boss.

End-to-end latency measurement
Tick-to-trade latency
Order-to-ack latency
Order-to-fill latency
p50/p95/p99/p99.9
CPU-cycle measurement
Cache-miss measurement
Branch-miss measurement
Allocation measurement
Context-switch measurement
Tail-latency analysis
Latency-budget analysis

Then optimize the complete system.

And one more thing I'd add: Assembly

I would not make assembly a giant separate programming phase.

Instead, whenever you optimize something, inspect what the compiler generated.

For example:

C++
 ↓
-O2 / -O3
 ↓
assembly
 ↓
CPU

Learn enough x86-64 to understand:

rax
rbx
rcx
rdx
rsp
rbp
rdi
rsi
r8-r15
call
ret
mov
lea
cmp
test
conditional jumps
lock
memory barriers
function calling convention

Then use:

Compiler Explorer
objdump
perf

to connect C++ → assembly → CPU performance.

That's much more useful than doing an isolated "learn assembly" phase.

So the final structure becomes
                    HFT C++ SYSTEM
                          │
       ┌──────────────────┼──────────────────┐
       │                  │                  │
  MARKET MICROSTRUCTURE  C++/MEMORY       ORDER BOOK
       │                  │                  │
       └──────────────────┼──────────────────┘
                          ↓
                    CONCURRENCY
                          ↓
                     CPU/CACHE
                          ↓
                       LINUX
                          ↓
                     NETWORKING
                          ↓
                  TICK-TO-TRADE
                          ↓
                      STRATEGY
                          ↓
                    EXECUTION
                          ↓
                       RISK
                          ↓
                     FAILURE
                          ↓
                   LATENCY TUNING
                          ↓
                 COMPLETE HFT SYSTEM
What I deliberately wouldn't add

I would not keep expanding this indefinitely with:

dozens of exotic order types
complicated quantitative strategies
advanced machine learning
huge numbers of LeetCode problems
every possible exchange protocol
every possible allocator
every possible lock-free algorithm

That would make the roadmap impressive-looking but less effective.

The sweet spot is roughly 125 meaningful implementations/experiments, but they all belong to one coherent system.

And the most important progression is:

Build it → measure it → find the bottleneck → understand why → optimize it → measure again.

That is the part that will make your HFT preparation genuinely strong rather than just a checklist of technologies.

so should we freeze it or do you still think it can be optimised more

I would freeze it now.

We have reached the point where further expansion would mostly add noise rather than interview value. The roadmap now has the right balance between:

C++
memory management
CPU/cache
concurrency
Linux
networking
market microstructure
order books
execution
risk
failure handling
latency measurement
system design

The important optimization I would make is not adding more topics, but changing how we execute the roadmap.

I would freeze the content at 12 phases
1.  Market Microstructure
2.  Order Book
3.  C++ Memory & Data Structures
4.  CPU & Cache
5.  Concurrency
6.  Linux / OS
7.  Networking
8.  Complete Tick-to-Trade System
9.  Trading Strategies
10. Execution Algorithms
11. Risk + Failure Engineering
12. Latency / Performance Engineering

And use the 125-ish programs as a progression, not as a checklist.

The rule for every program

Each one should answer four questions:

1. Can I implement it?
2. Can I explain how it works?
3. Can I measure its performance?
4. Can I explain WHY one implementation is faster?

For example, don't merely implement:

SPSC ring buffer

Instead:

mutex queue
      ↓
spinlock queue
      ↓
SPSC ring buffer
      ↓
benchmark
      ↓
perf
      ↓
cache analysis
      ↓
memory-ordering analysis
      ↓
explain why SPSC wins

That's what turns it into HFT preparation.

One final adjustment

I would also make DSA/coding a parallel track, rather than adding it to the 125-program roadmap.

So your overall preparation is:

                 HFT PREPARATION
                       │
          ┌────────────┴────────────┐
          │                         │
      HFT SYSTEM                INTERVIEW CODING
          │                         │
     12 phases                 C++ DSA
          │                   Algorithms
     125 programs             Problem solving
          │
     Benchmarking
          │
     Performance
          │
     System design

That prevents the HFT project from becoming bloated with unrelated LeetCode exercises.
