# Changelog

All notable changes to the Dedupe plugin will be documented in this file.

## [2026.10.30] — 2026-10-30

### Security
- Fixed XSS vulnerability — all file paths are now HTML-escaped before rendering
- Fixed path traversal in delete API — uses `realpath()` to canonicalize paths
- Fixed potential command injection via PID file — PIDs are validated as numeric
- Added `Content-Type: application/json` headers to all API responses
- All shell arguments use `escapeshellarg()` throughout

### Added
- **Deep Verification** — byte-by-byte comparison via `cmp` after hashing (previously the checkbox existed but did nothing)
- **Minimum File Size filter** — skip files smaller than a configurable threshold (default: 1 MB)
- **Exclusion Patterns** — comma-separated directory names to skip (default: `.Recycle.Bin, @eaDir, .Trash`)
- **Persistent Results** — scan results survive page navigation; automatically restored on page load
- **Selection Helpers** — "Select All Duplicates" / "Deselect All" buttons with live selection summary
- **KEEP badge** — first file in each group is visually marked as the keeper
- **Better delete confirmation** — shows file count and estimated space savings
- **AJAX error handling** — all API calls now show user-visible error messages on failure
- **Structured logging** — timestamps, file counts, timing, and space statistics in scan logs

### Fixed
- Scan script handles empty directories and zero-candidate scenarios gracefully
- Progress calculation no longer divides by zero
- Status file writes are atomic (temp file + rename) to prevent corrupt JSON
- Removed broken PNG icon placeholder (was a CSS comment, not a real image)
- Page-load status check handles `idle` state without starting unnecessary polling
- Uninstall hook now kills running scans before cleanup
- Uninstall cleans up all temp files including `.tmp` variants

## [2026.04.01] — 2026-04-01

### Fixed
- **CRITICAL**: Hardcoded plugin name in uninstall script to prevent XML CDATA variable escaping from wiping entire plugins directory
- Uninstallation cleanup now removes the tool from webGUI immediately

### Added
- Full SHA hash suite (SHA-1, SHA-224, SHA-256, SHA-384, SHA-512)
- Unraid native notification system integration for scan completion
- Space reclamation tracking in logs

### Changed
- Generalized all comments and UI text for public release
- Unified plugin icons and added description for Plugins page

