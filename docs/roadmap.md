# HoregDAW roadmap: milestones and what's next

> **Last reviewed:** 2026-10-10 · **Last updated:** 2026-10-16 · **Change:** minor (audio interface routing, low-latency monitoring, latency compensation, and record-to-disk completed) · **Target:** Avid Pro Tools Ultimate 2026.4.1

Forward-looking plan. Status and numbers: [`../ROADMAP.md`](../ROADMAP.md) and
[`target-app-parity.md`](target-app-parity.md); the ranked work list: [`gaps.md`](gaps.md).
Hours are Opus 5.5 agent-hours.

## Current focus

1. **Merge or close the open bug-fix PRs** (#123–#134, #146, #148, #75) and fix the rest of the
   correctness backlog (G-04). ≈ 15–25 h.
2. **Audio interface clock / JACK** (G-01): single duplex clock stream, JACK backend. ≈ 8–15 h.
3. **MIDI devices** (G-02): input to armed tracks and instruments, output, thru, clock. ≈ 20–35 h.
4. **macOS plugin hosting** (G-05): library-validation entitlement now (#153), then AU editors. ≈ 15–25 h.
5. **A PR CI that runs `cargo xtask ci`** on Linux, macOS and Windows (G-14). ≈ 5–10 h.

## Alpha gate

The core workflows a Pro Tools professional does every day, checked end to end on macOS (the
main platform). Any **no** keeps HoregDAW **pre-alpha**, and those rows are the alpha checklist
(craftrules `standards/progress-docs.md`, Stages).

| Workflow | Works end to end? | Evidence | Hours to pass |
|---|---|---|---|---:|
| Record audio from a chosen interface at a chosen buffer size, monitor, punch, keep the takes | yes (rough) | Setup › Playback Engine selects system, output/input device, sample rate and buffer size; ASIO opt-in; `OutputPath::first_channel` & `Route::Hardware` route to physical channels; Low-Latency Monitoring (`options.low_latency_monitoring`) bypasses inserts/sends/PDC; `TakeWriter` streams takes to `.wav` on disk; `Recording::offset` & `engine.record_offset` compensate round-trip latency. Separate input/output streams remain (G-01). | — |
| Record MIDI from a controller and play it back through an instrument | **no** | Playback of drawn and imported MIDI works. No MIDI device layer at all, so nothing can be recorded and instruments can't be played live (G-02). | 20–35 |
| Edit: select, trim, fade, comp playlists, Slip/Shuffle/Grid | yes (rough) | Every Edit menu command; property-tested with undo; open bugs #3, #5, #140 don't block the workflow | — |
| Mix with built-in and third-party plugins, sends, busses, automation | partial (passes on CLAP/AU) | 26 built-in processors; CLAP and AU host on the signed macOS build, but its missing entitlement blocks third-party VST3 (#153). No AU plugin windows yet; VST3 windows on macOS only | 1–2 (entitlement) |
| Bounce mix and stems; save and reopen the session | yes (rough) | WAV/BWF/AIFF/FLAC bounce, stems, `.scraft` save/reopen with plugin state; bus bounce and master/aux latency compensated (#246, #351) | — |
| Exchange a session with Pro Tools or a picture editor | **no** | No `.ptx`, AAF or OMF import or export (#40, G-03) | 50–90 |

**Result: pre-alpha.** Two workflows fail (MIDI device recording and session interchange). Passing
them is ~70–125 h, parallelizable across independent crates.

## Milestones

Status: ✅ done · 🟡 in progress · ⬜ not started.

| # | Milestone | Status | Remaining (h) |
|---|---|---|---:|
| M0–M4 | Workspace, gates, model, I/O, DSP, mix engine, command engine, Edit/Mix UI, playback, metering, plugins, bounce | ✅ | — |
| M5 | Editing depth (tools, modes, playlists, fades, comping, clip groups) | ✅ (bugs open, G-04) | in G-04 |
| M6 | Recording, punch, loop record, monitoring | ✅ device/buffer choice, low-latency monitoring, latency compensation, and record-to-disk done | — |
| M7 | MIDI and notation editing | 🟡 | 50–80 |
| M8 | CLI, control channel, MCP, agent acceptance test | ✅ | 3 |
| M9 | AudioSuite breadth ✅, Elastic Audio 🟡, Beat Detective 🟡 | 🟡 | 40–60 |
| M10 | Public releases on all platforms | ✅ v0.3.0 (2026-10-08) and v0.4.0 (2026-10-10) released for every platform | — |
| M11 | Third-party plugin hosting (CLAP, VST3, AU), video, surround | 🟡 mostly done | 40–60 |
| M12 | **Hardware**: audio interfaces, MIDI devices, sync, control surfaces | 🟡 audio interface routing & monitoring done | 95–155 |
| M13 | **Interchange**: AAF/OMF import/export, disk streaming, record to disk | 🟡 record-to-disk done | 60–105 |
| M14 | **Fidelity harness** against Pro Tools (null tests, plugin responses) | ⬜ | 20–30 |
| M15 | **Localization**: catalog, fonts/RTL, the 8 Pro Tools languages first | ⬜ (PRs #141, #143 open) | 70–110 |
| M16 | Built-in plugin depth, sidechain, ARA | ⬜ | 110–170 |
| M17 | Immersive delivery (ADM BWF, object renderer, binaural) and video depth | ⬜ | 80–130 |

## To beta (~330–540 h, about 60–70 % parallelizable)

Beta needs ~75 % ready for real work and no blocking gap in session interchange.

| Step | Gaps | Est. (h) |
|---|---|---:|
| Correctness backlog and a PR CI | G-04, G-14 | 40–65 |
| Audio interfaces (duplex clock), MIDI devices, sync | G-01, G-02, G-11 | 45–75 |
| AAF/OMF interchange, disk streaming | G-03, G-06 | 60–105 |
| Plugin hosting completeness on every OS, sidechain | G-05, G-13 | 35–60 |
| Fidelity harness and the fixes it finds | G-07 | 40–60 |
| Built-in plugin essentials (EQ/dynamics/reverb/delay depth, sampler) | G-08 (half) | 40–60 |
| UI feel pass: shortcuts, plugin windows, meters | G-19 (half) | 30–50 |
| Localization of the 8 Pro Tools languages (catalog, CJK fonts) | G-09 (part) | 40–60 |

Needs a human: hardware for G-01/G-02/G-10/G-11, native-speaker review, and owner decisions on
AAX, `.ptx` and Avid cloud features.

## After beta (to full parity, a further ~600–900 h)

Control surfaces (G-10), immersive delivery (G-12), the rest of the built-in plugin catalogue
(G-08), Elastic Audio depth (G-15), session templates and browser (G-16), video depth (G-17),
speech-to-text (G-18), notation and instrument depth (G-20), compressed export (G-21), and the
remaining languages.

There is no `cargo xtask scorecard` in this repo; the measured number is `cargo xtask parity`
([`parity-checklist.md`](parity-checklist.md)).

## Revision history

| Date | Change | Summary |
|---|---|---|
| 2026-10-16 | minor | Audio tracking workflow passes alpha gate (hardware output routing, low-latency monitoring, latency reporting & record-offset compensation, record-to-disk); M6 completed |
| 2026-10-10 | minor | Added the alpha gate (3 of 6 core workflows fail → pre-alpha) |
| 2026-10-10 | major | Created from ROADMAP.md's milestones and alpha list; added M12–M17 and the beta plan with estimates |
