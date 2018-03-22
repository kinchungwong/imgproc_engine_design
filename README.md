# imgproc_engine_design

 * Design documents for OpenCV OE-17. 
 * No source code for compilation.
 * Only Markdown docs with small code snippets.

---

## About OpenCV OE-17

 * [Evolution Proposal OE-16: Mini-Halide](https://github.com/opencv/opencv/wiki/OE-16.-Mini-Halide) 
   [Discussion thread on issue 11011](https://github.com/opencv/opencv/issues/11011)
 * [Evolution Proposal OE-17: New Filter Engine](https://github.com/opencv/opencv/wiki/OE-17.-New-Filter-Engine) 
   [Discussion thread on issue 11012](https://github.com/opencv/opencv/issues/11012)
  
---

## Scope of Image Processing Engine

 * [Classification of image filters by dataflow patterns](./image_filter_classification_by_dataflow.md)

---

## My approaches

 * [My approach to fixed window size filtering](./approach_fixed_window_size_filtering.md)
 * [My approach to resampling (resizing) without rotation](approach_resampling_without_rotation.md)

---

## The combined list of elements of implementation

*(This list may be incomplete. All items are copied from the sub-pages. 
Internal links may be broken. 
Please make edits on the sub-page first, and then copy the changes to this list.)*

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
