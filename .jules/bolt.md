## 2026-03-30 - Planar Audio Buffer Hoisting in DSP Loops
**Learning:** Indexing channel buffers via `io.first().and_then(|b| b.get(n))` inside per-frame audio processing loops incurs bounds checks and Option destructuring overhead per sample.
**Action:** Split planar channel buffer slices (`io.split_at_mut`) outside the frame loop to obtain direct slice handles (`&mut v[..frames]`) before iterating over frames.

## 2026-03-31 - Active Biquad Section Filtering in Multi-Band EQs
**Learning:** Fixed-size cascaded biquad structures (e.g., 9 biquad sections in Eq7) execute full second-order IIR calculations for every band per sample even when bands are disabled or set to 0 dB gain (`Coeffs::IDENTITY`).
**Action:** Maintain an active section index array during `redesign()` to only process non-identity biquad sections in the per-sample audio filtering loop.
