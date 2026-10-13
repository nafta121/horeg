## 2026-03-30 - Planar Audio Buffer Hoisting in DSP Loops
**Learning:** Indexing channel buffers via `io.first().and_then(|b| b.get(n))` inside per-frame audio processing loops incurs bounds checks and Option destructuring overhead per sample.
**Action:** Split planar channel buffer slices (`io.split_at_mut`) outside the frame loop to obtain direct slice handles (`&mut v[..frames]`) before iterating over frames.
