## 2026-03-30 - Planar Audio Buffer Hoisting in DSP Loops
**Learning:** Indexing channel buffers via `io.first().and_then(|b| b.get(n))` inside per-frame audio processing loops incurs bounds checks and Option destructuring overhead per sample.
**Action:** Split planar channel buffer slices (`io.split_at_mut`) outside the frame loop to obtain direct slice handles (`&mut v[..frames]`) before iterating over frames.

## 2026-03-30 - Bypassing Identity Biquads and Block-Processing Cascaded Filters
**Learning:** Cascaded biquad filters (e.g. 9-band EQ) often contain multiple inactive/identity sections. Processing sample-by-sample across all sections prevents SIMD vectorization and wastes cycles on identity math.
**Action:** Fast-path check `b.is_passthrough()` and block-process chunk slices (`b.process_block(seg)`) per active section to allow loop vectorization and skip inactive bands.
