# Dedupe — Unraid Deduplication Plugin

[![Unraid](https://img.shields.io/badge/Unraid-6.9%2B-orange?style=flat-square&logo=unraid)](https://unraid.net)
[![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](LICENSE)

A high-performance deduplication tool for Unraid OS. Scan your shares for identical files using hardware-accelerated SHA/MD5 hashing and reclaim wasted array space with ease.

![Dedupe Screenshot](docs/screenshot.png)

## Features

- **Smart Scanning** — Files are grouped by size first, then only size-duplicates are hashed, minimizing disk I/O
- **Multiple Hash Algorithms** — MD5, SHA-1, SHA-224, SHA-256, SHA-384, SHA-512
- **Deep Verification** — Optional byte-by-byte comparison to eliminate any chance of hash collision false positives
- **Minimum File Size Filter** — Skip tiny config/metadata files to focus on meaningful duplicates
- **Exclusion Patterns** — Skip directories like `.Recycle.Bin`, `@eaDir`, `.Trash`
- **Background Scanning** — Scans run in the background with pause/resume/cancel support
- **Real-Time Progress** — Live progress bar, status updates, and scrolling log output
- **Persistent Results** — Navigate away and come back — your results are still there
- **Space Reclamation Tracking** — See exactly how much space you'll save before deleting
- **Unraid Notifications** — Get notified when scans complete via Unraid's native notification system
- **Secure by Design** — Path traversal protection, XSS prevention, input validation throughout

## Installation

### From Community Applications (Recommended)
1. Open the **Apps** tab in Unraid
2. Search for **Dedupe**
3. Click **Install**

### Manual Installation
1. Copy `dedupe.plg` to your Unraid flash drive:
   ```bash
   # From your Unraid terminal:
   wget -O /boot/config/plugins/dedupe.plg https://raw.githubusercontent.com/greenyc20/unraid-dedupe/main/dedupe.plg
   installplg /boot/config/plugins/dedupe.plg
   ```
2. The plugin appears under **Utilities → Dedupe Tool** in the webGUI

## Usage

1. Navigate to **Utilities → Dedupe Tool** in the Unraid webGUI
2. Select a target share or leave on "All User Shares"
3. (Optional) Expand **Advanced Settings** to configure:
   - Hash algorithm
   - Minimum file size
   - Directory exclusions
   - Deep verification
4. Click **Start Scan**
5. Review duplicate groups — the first file in each group is marked **KEEP** by default
6. Adjust selections as needed, then click **Delete Selected Duplicates**

## How It Works

```
Step 1: Find all files in the target path
         ↓
Step 2: Group files by size (files with unique sizes can't be duplicates)
         ↓
Step 3: Hash only the size-duplicate candidates (smart filtering)
         ↓
Step 4: Group by hash → these are your duplicates
         ↓
(Optional) Deep verify with byte-by-byte comparison (cmp)
```

This approach is dramatically faster than hashing every file because typically only 5-15% of files share a size with another file.

## Requirements

- Unraid OS 6.9.0 or later
- No additional dependencies (uses built-in coreutils for hashing)

## Security

This plugin runs with root privileges (as all Unraid plugins do). Security measures include:

- **Path traversal protection** — All file paths are resolved via `realpath()` and validated against `/mnt/` prefix
- **XSS prevention** — All user-supplied data is escaped before DOM insertion
- **Input validation** — Hash methods are whitelisted, PIDs are validated as numeric, file sizes are sanitized
- **CSRF protection** — Handled automatically by Unraid's Dynamix framework
- **No command injection** — All shell arguments use `escapeshellarg()`

## Support

- **Unraid Forums**: [Support Thread](https://forums.unraid.net/) *(link TBD)*
- **Issues**: [GitHub Issues](https://github.com/greenyc20/unraid-dedupe/issues)

## License

MIT License — see [LICENSE](LICENSE) for details.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history.

