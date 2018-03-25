# Style helper library for Explicit SIMD programming in C++

 * Link to this file: [explicit_simd_style_helper_lib.md](./explicit_simd_style_helper_lib.md)

---

## Current source code home page

 * https://github.com/kinchungwong/simd_style_helper
 * Currently these source code do not have a license statement (which puts them in 
   private exclusive license).
 * Once discussion with OpenCV begins, I as the author of this code will relicense
   them under OpenCV license, or a license compatible with OpenCV. (The reason not 
   to do it now is to wait for OpenCV's permission.)
 
---

## Note on existing Explicit SIMD template libraries based on recent C++ specification revisions

 * There are many similar C++11/14/17 template libraries already published.
 * Readers are encouraged to take a look at alternatives, and then choose a
   evaluation criteria best for a certain application or specific type of 
   algorithms.
   
---

## List of existing libraries 

*(This information is incomplete - updates welcome.)*

 * QuickVec C++
   * Author: Matthew Kellogg
   * Project home page: https://www.andrew.cmu.edu/user/mkellogg/15-418/
   * Source code: https://bitbucket.org/kellogg92/quickvec
   
 * UME::SIMD
   * A component of UME (Unified Multicore Environment)
   * Main project home page: https://gain-performance.com/ume/
   * Source code: 
     * UME::SIMD component: https://github.com/edanor/umesimd
	 * UME::Vector component: https://github.com/edanor/umevector
   * Attributions:
     * This piece of code was developed as part of ICE-DIP project at CERN.
	 * "ICE-DIP is a European Industrial Doctorate project funded by the European Community's 7th 
	    Framework programme Marie Curie Actions under grant PITN-GA-2012-316596".

---

## Note on compiler vendor specificity

 * Much of this library was prototyped on Microsoft Visual C++ 2017 compiler, with 
   full optimization, targeting "x64" platform and "Release" configuration.
   
---

## Design goals

 * To provide a very thin wrapper over explicit SIMD programming (intrinsics
   targeting a specific CPU generation, with fixed SIMD width, and an instruction 
   set where some instructions are not available to earlier CPU generations)
   
 * The purpose of the very thin wrapper is to reduce the eye strain when reading
   SIMD-intensive source code.
   
 * Also, it encourages the definition of meaningful macros, consisting of 
   repetitions of short fragments of SIMD intrinsics. These macros will be 
   based on C++ templates.
   
 * To facilitate use of narrower data types, such as 8-bit and 16-bit integers,
   both signed and unsigned. These narrower data types allow more units of data
   to be processed within the memory bandwidth budget.
   
---




