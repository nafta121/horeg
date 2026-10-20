## 2026-03-30 - Planar Audio Buffer Hoisting in DSP Loops
**Learning:** Indexing channel buffers via `io.first().and_then(|b| b.get(n))` inside per-frame audio processing loops incurs bounds checks and Option destructuring overhead per sample.
**Action:** Split planar channel buffer slices (`io.split_at_mut`) outside the frame loop to obtain direct slice handles (`&mut v[..frames]`) before iterating over frames.

## 2026-03-30 - Skipping Inactive Biquad Bands in EQ Processing Loops
**Learning:** Cascaded EQ plugins (like EQ 7-Band) evaluate all filter sections per sample, even when band gains are 0 dB / identity. Inactive biquad calculations incur unnecessary float ops per sample per channel.
**Action:** Track active section indices (`active_sections`) whenever equalizer parameters change or filter coefficients are redesigned, and iterate only over active sections in sample loops.
