# HOREGDAW

HOREGDAW is a clean-room, open-source digital audio workstation built in pure Rust for recording, editing, mixing, and automation.

## Overview

HOREGDAW follows the familiar Pro Tools-style workflow: edit window, mix window, MIDI editing, automation, plugins, recording, and session management, all designed to run natively on macOS, Windows, Linux, FreeBSD, and in the browser.

## Project naming

The project name is now HOREGDAW in the user interface and documentation.

## Getting started

```bash
git clone https://github.com/nafta121/horeg.git
cd horeg
cargo run --release -p horegdaw -- --demo
```

## Status

HOREGDAW is in active development. The core workspace is built in Rust and is designed to be modular, layered, and scriptable from the command line and AI control interfaces.

## License

HOREGDAW is distributed under the MIT or Apache-2.0 licenses.
