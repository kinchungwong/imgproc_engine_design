# Classification of image filters by dataflow patterns

---

## About this nomenclature

The names of each classification have been truncated to make them more succint. 
Under each section, a more detailed explanation ("what it is / is not") is given.

---

## Overview

 * "Fixed window size filtering" 
   * Fixed-size convolution filters (not necessarily linear or separable)
   * Fixed-size morphological operations
 * "Resampling without rotation" 
   * Image resize, with or without interpolation
 * "Tile-based transposition readout" 
   * Rotation by 90, 180, 270 degrees.
 * "Remapping" 
   * Fine rotation
   * Affine
   * Perspective
   * Other non-linear transformations, such as LogPolar, FishEye

---

## Fixed window size filtering

Also known as:

Properties:

 * Non-resizing (If input is Width x Height, output has same size.)
 * Non-rotating ("Up means up". Specifically, the direction of the two axes are preserved.)
 * Each output pixel at ```(i, j)``` corresponds to a fixed-size input window, whose position
   is relative to ```(i, j)```. (It need not be centered at ```(i, j)```.)

Some well-known sub-types:

 * Convolution (also known as Stencil), <br>where output pixel is weighted combination of pixels from input window
   * Some convolutions satisfy a "separable" property
   * 
 * Morphological filtering
 * Ordinal filtering (order-statistic filtering)
   * Erosion, or its grayscale counterpart, Min filter
   * Dilation, or its grayscale counterpart, Max filter
   * Median filter
   * Ordinal clamped denoise filter

Some well-known approaches to this problem space:

 * "Halide" (from MIT CSAIL)
 * Most CUDA programmers can write such code; <br>Coalescing is handled pretty well by newer GPU models and compiler versions.

My approaches to this problem space:

 * [My approach to fixed window size filtering](./approach_fixed_window_size_filtering.md)

---

## Resampling without rotation

Also known as:

 * Image resize (upsampling, downsampling)

Properties:

 * There is a resizing factor (not necessarily integer-valued) which guides the resampling
   * For each output pixel ```(xo, yo)```, it is conceptually associated with a location 
     in the input image roughly ```(xi = a * xo + b, yi = c * yo + d)```, where 
	 ```(xi, yi)``` are not necessarily integer-valued.
 * With or without interpolation. <br>If interpolation is involved, the input window size
   is larger than (1 by 1), and its exact size may fluctuate depending on the output pixel 
   coordinates.

Properties which are found in typical implementations:

 * Most implementations tend to be linear and separable.

Some well-known approaches to this problem space:

 * GPU texture memory and interpolation unit

My approaches to this problem space:
 
 * [My approach to resampling (resizing) without rotation](approach_resampling_without_rotation.md)

---
 
## Transpositions

Also known as:

 * Rotation by 90, 180, or 270 degrees
 
Properties which are found in typical implementations:

 * Typically, the output size is same as input size.
 * Typically, interpolation is not involved.
   * When interpolation is not involved, it is not necessary to perform 
     arithmetics on pixel values. Thus, the implementation becomes pixel
	 copying
 * A naive implementation tend to be cache-unfriendly.
 * However, optimized implementations exist which exploit all of:
   * SIMD (vectorization),
   * Multi-core processing (parallelization), and 
   * Cache locality, by applying the tiling technique

---

## General image resampling

Also known as:

 * Remapping, in OpenCV terminology, for example ```cv::remap()```

Properties which are found in typical implementations:

 * Most practical examples of general image resampling belong to a class of 
   mathematical function known as ["diffeomorphism"](https://en.wikipedia.org/wiki/Diffeomorphism)
   * Some discontinuities may exist, but the mapping is invertible and smooth in most places.

Examples:

 * Affine coordinate transformation
   * Image rotation by an angle other than 0, 90, 180, or 270 degrees
 * Perspective coordinate transformation
 * Camera lens or mirror distortion correction
   * Fish-eye correction
 * Polar transformation 
   * Examples in OpenCV: ```cv::linearPolar```, `cv::logPolar```

Some well-known approaches to this problem space:

 * GPU texture memory and interpolation unit
 * Mip-mapping technique
 * Full-scale anti-aliasing (FSAA)

---
