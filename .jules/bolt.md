## 2026-03-30 - Planar Audio Buffer Hoisting in DSP Loops
**Learning:** Indexing channel buffers via `io.first().and_then(|b| b.get(n))` inside per-frame audio processing loops incurs bounds checks and Option destructuring overhead per sample.
**Action:** Split planar channel buffer slices (`io.split_at_mut`) outside the frame loop to obtain direct slice handles (`&mut v[..frames]`) before iterating over frames.

## 2026-03-31 - Hoisting dB-to-Gain Computations and Slice Resolution in Audio Loops
**Learning:** Calling functions that compute linear gains from decibels (`db_to_gain`, `10.0.powf`) or resolving channel slices inside per-sample audio rendering loops executes millions of redundant transcendental operations and bounds checks per block.
**Action:** Pre-compute linear gain values (`db_to_gain`) and resolve tap slice handles (`src[start..end]`) outside per-sample frame loops, allowing SIMD vectorization and fast-pathing unity or static gain blocks.
