# Explicit SIMD image filtering performance

 * Link to this page: [explicit_simd_image_filtering_performance.md](./explicit_simd_image_filtering_performance.md)
 
---

## Applicability

 * Focus on ```uint8_t``` and ```uint16_t``` as pixel numeric precision (depth)
 * Intermediate results in ```uint32_t``` or ```int32_t``` or ```float``` will be discussed
 * Focus on desktop CPUs, with x86-64 compatible architectures
   * Out-of-order, superscalar, speculative execution
   * Assumes that the CPU is capable of allowing a large number of in-flight instruction executions
     * Reorder buffer, 
	 * Register renaming, 
	 * Instruction retirement unit
	 * Assumed to be on the order of a hundred of in-flight executions
 * Register spill is inevitable, but assume that scratch memory formed from aligned stack space can have good performance.
 * Assumes a very competent C++ optimizing compiler.

---

## Intended audience

The reader is assumed to be familiar with low-level C++, explicit SIMD using vendor-specific intrinsics, 
disassembly, x86-64 compatible architecture, micro-benchmarking.

Much of the discussion will go into disassembly in an attempt to find explanations for certain 
performance phenomenons in explicit SIMD programming.

---

## A performance heuristic view of memory hierarchy

**The performance heuristic, from fastest to slowest:**

 * CPU SIMD registers,
 * Heavily reused aligned memory space from stack space 
   * The aligned local stack space is created when entering a function's prologue, and cleaned up in the epilogue.
   * SIMD values can be loaded ans saved using the aligned instructions ```MOVDQA``` (```_mm_load_si128```)
   * Because they are located on the stack space, their addresses are hard-coded relative to the stack pointer.
     * Example: (from MSVC disassembly)
 * Heavily reused row buffers
   * [Dynamically-sized memory pool for row buffers](./row_buffers.md)
 * Consecutive reading and writing of rows from/to 2D matrices (images)
 * Unpredictable memory access

**Justification fot this heuristic view:**

The speed of within-CPU register operations does not need explanation.

However, the classification of memory into four performance levels is merely a heuristic. 
This heuristic has no counterpart in CPU architecture. It is well-known that CPUs have L1, 
L2 and possibly L3 caches in addition to DRAM. Such details are managed transparently by 
the CPU. Therefore, the heuristic view don't map exactly to these CPU components.

Instead, the heuristic correlates more strongly with: 

 * The ability and probability for certain pieces of data to stay within L1, or L2
 * The ability for that piece of data to resist eviction
 * The ability to leverage store-to-load forwarding, for SIMD loads and stores to 
   aligned memory space, within tight SIMD loops
 * The ease at which the CPU's cache prefetch mechanism can correctly load the next 
   cache lines ahead of time

---
