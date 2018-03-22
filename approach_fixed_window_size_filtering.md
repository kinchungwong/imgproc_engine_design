# My approach to fixed window size filtering

## Related

 * [Classification of image filters by dataflow patterns](./image_filter_classification_by_dataflow.md)

---

## Elements of implementation

 * Dynamically-sized memory pool for row buffers
 * An outermost loop responsible for:
   * "scheduling", 
   * "cascading", and 
   * "multi-worker parallelization"
 * SIMD programming styles
   * Explicit SIMD
     * Raw (using vendor-specific intrinsics, or vendor-agnostic wrappers)
     * Syntactically improved, to improve code maintenance and user-friendliness
   * Row-level vectorization without explicit SIMD

---

## Dynamically-sized memory pool for row buffers

 * [Dynamically-sized memory pool for row buffers](./row_buffers.md)

---

## Design of the outermost loop

---

## Explicit SIMD programming

 * [Explicit SIMD image filtering performance](./explicit_simd_image_filtering_performance.md)

---

## Stylistically improved explicit SIMD programming

---

## Row-level vectorization without explicit SIMD

---
