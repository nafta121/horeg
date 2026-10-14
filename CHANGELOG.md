# Changelog

## v0.1.0 — HOREGDAW (draft)

Summary

- Project display name changed from "HoregDAW" to "HOREGDAW" in the UI and documentation.
- Internal packaging and executable identifiers updated to HOREGDAW where appropriate (desktop app, CLI, Windows installer WiX file, Linux .desktop / AppStream / MIME metadata).
- README, assets/app-icon README and packaging web README updated to use HOREGDAW.
- apps/horegdaw package renamed to `horegdaw` (Cargo.toml) and apps/horegdaw-cli renamed to `horegdaw-cli`.
- Desktop and packaging metadata updated: packaging/windows/horegdaw.wxs -> now references HOREGDAW, packaging/linux desktop/metainfo/mime updated to HOREGDAW ids and binary names.

Notes and migration guidance

- Executable names changed:
  - horegdaw -> horegdaw
  - horegdaw-cli -> horegdaw-cli

  After installing a packaged build, run `horegdaw --version` and `horegdaw-cli --help` as the new commands.

- Many internal crate/package names remain `horegdaw-*` (engine, dsp, ui-egui, etc.). Only the user-facing app package and CLI were renamed in this pass. If you want to rename all crate names, that requires a coordinated workspace refactor.

- Some environment variables and configuration keys still refer to SOUNDCRAFT in code and docs (for example `SOUNDCRAFT_NO_PREFS` and config directories). I intentionally left these in place where they control internal behavior to avoid breaking existing runtime preferences and CI, but we can rename them in a follow-up if desired. Search the repo for `SOUNDCRAFT_` to find remaining usages.

- Packaging artifacts were not built as part of this change. The release is a branding & packaging metadata update and should be published as a draft or prerelease until you:
  - Build and attach platform artifacts (Windows MSI, macOS DMG, Linux AppImage/flatpak, web zip).
  - Verify Windows WiX packaging with the updated exe names.
  - Verify Linux desktop entries and AppStream metadata on a CI runner.

Files changed (high level)

- README.md
- AGENTS.md (mentioned HOREGDAW already)
- apps/horegdaw/Cargo.toml -> apps/horegdaw/Cargo.toml (name changed to `horegdaw`)
- apps/horegdaw-cli/Cargo.toml -> (name changed to `horegdaw-cli`)
- apps/horegdaw/src/main.rs (title and window/app strings)
- packaging/windows/horegdaw.wxs (renamed identifiers inside to HOREGDAW)
- packaging/linux/ai.storyteller.horegdaw.desktop -> updated to use HOREGDAW values
- packaging/linux/ai.storyteller.horegdaw.metainfo.xml.in -> updated ids/names
- packaging/linux/ai.storyteller.horegdaw.mime.xml -> updated mime type and comment
- assets/app-icon/README.md
- packaging/web/README.md

Suggested release metadata (recommended)

- Tag: v0.1.0
- Release title: HOREGDAW v0.1.0
- Release notes: Use the text above (the contents of this changelog entry). It explains what's changed and what remains.
- Draft: true (recommended) — the release should be a draft until packaging artifacts (installers, zips) are built and attached and the platform-specific changes are verified.
- Prerelease: false (unless you prefer marking it as a prerelease for CI/testing)

How to publish the release (examples)

Using GitHub CLI (recommended):

1. Tag and push the tag (locally):

   git tag -a v0.1.0 c9a645399fff8680e92a7cd3295298c963c2c4b9 -m "HOREGDAW v0.1.0"
   git push origin v0.1.0

2. Create a draft release with the prepared notes:

   gh release create v0.1.0 --title "HOREGDAW v0.1.0" --notes-file CHANGELOG.md --draft

Using the REST API (with GITHUB_TOKEN):

curl -H "Authorization: token $GITHUB_TOKEN" \
  -d '{"tag_name":"v0.1.0","target_commitish":"main","name":"HOREGDAW v0.1.0","body":"'"$(sed ':a;N;$!ba;s/"/\\"/g' CHANGELOG.md)"'", "draft": true, "prerelease": false }' \
  https://api.github.com/repos/nafta121/horeg/releases

Next steps I can take for you

- If you provide a GitHub token (with repo scope) I can create the release (draft) for you.
- I can also push a git tag for you if you provide a token that allows creating refs, or you can run the `git tag` + `git push` commands locally.
- I can build packaging artifacts (if you want instructions / CI setup) or add a CI workflow that builds artifacts and publishes them to the release automatically.

