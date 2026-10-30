# Dedupe Plugin — Roadmap & Architecture Review

This document contains the original code review, security audit, and 4-phase development roadmap for the Unraid Dedupe plugin.

---

## 🏗️ Architecture Overview

```mermaid
flowchart TD
    A["Dedupe.page<br/>(Menu Entry)"] --> B["Dedupe.php<br/>(UI + JavaScript)"]
    B -->|"AJAX + CSRF Token"| C["api.php<br/>(PHP Backend)"]
    C -->|"Start"| D["scan.sh<br/>(Background Scanner)"]
    C -->|"Read"| E["/tmp/dedupe_status.json"]
    C -->|"Read"| F["/tmp/dedupe_scan.log"]
    D -->|"Write (atomic)"| E
    D -->|"Append"| F
    D -->|"Complete"| G["notify script<br/>(Unraid Alerts)"]
    C -->|"Delete"| H["File Deletion<br/>(with realpath validation)"]
```

---

## 4-Phase Roadmap

### ✅ Phase 1: Critical Security (Completed in v2026.10.30)
- [x] **#1 CSRF Token Compatibility**: Verified Unraid Dynamix auto-appends CSRF tokens on all `$.post()` requests.
- [x] **#2 XSS Prevention**: Added text-node DOM escaping (`escapeHtml()`) across all file paths and data tables.
- [x] **#3 Path Traversal**: Enforced `realpath()` validation requiring targets to resolve strictly inside `/mnt/`.
- [x] **#4 Command Injection via PID**: Enforced numeric-only regex (`^\d+$`) and `/proc/$pid` verification before invoking any process kill/stop commands.

### ✅ Phase 2: Core Bug Fixes (Completed in v2026.10.30)
- [x] **#5 Status Check on Page Load**: Added jQuery ready handler that correctly manages `idle`, `running`, `paused`, and `complete` states.
- [x] **#6 Empty Results Handling**: Gracefully handles zero-file or zero-duplicate scans without throwing script errors.
- [x] **#7 Deep Verification**: Fully implemented byte-by-byte comparison (`cmp -s`) when enabled, removing hash-collision false positives.
- [x] **#8 Progress Bar Division-by-Zero**: Added guards in `awk` when potential candidate count is 0.
- [x] **#9 Atomic Status Writes**: Script writes to `.tmp` files before renaming to eliminate corrupted JSON reads.
- [x] **#10 Icon Overhaul**: Embedded official FontAwesome clone icon and created clean 96×96 Unraid-orange icon for Community Applications.

### ✅ Phase 3: Core Usability (Completed in v2026.10.30)
- [x] **#11 Minimum File Size Filter**: Dropdown to skip files under 1 KB, 100 KB, 1 MB (default), 10 MB, 100 MB.
- [x] **#13 Path Exclusions**: Comma-separated exclude filter for `.Recycle.Bin`, `@eaDir`, `.Trash`, etc.
- [x] **#14 Persistent Results**: Results cached in `/tmp/dedupe_status.json` so navigating away doesn't lose your scan.
- [x] **#20 AJAX Error Handling**: Added `.fail()` handlers to provide user feedback when requests fail.
- [x] **#21 Proper HTTP Headers**: Standardized `Content-Type: application/json; charset=utf-8` on API endpoints.

---

### ✅ Phase 4: Polish & Advanced Features (Completed in v2026.10.31)

- [x] **#12 File Extension Filters**: Include (`mkv, mp4, iso`) and Exclude (`nfo, srt, tmp`) extensions.
- [x] **#15 Smart "Keep" Selection**:
  - Keep Oldest (by modification date `mtime`)
  - Keep Newest
  - Keep by Preferred Path/Disk Pattern (e.g. prioritize keeping `/media/` over `/downloads/`)
  - Keep Shortest Path
- [x] **#16 Enhanced Delete Confirmation & Safety**:
  - Detailed confirmation with preview of files targeted for deletion and exact reclaimable bytes.
- [x] **#17 Log Management & Size Limits**:
  - Log rotation caps `/tmp/dedupe_scan.log` at 1 MB / last 1000 lines.
- [x] **#18 Scheduled Scans (Cron)**:
  - Daily, Weekly, or Monthly automated scans via `/etc/cron.d/` with notification summaries.
- [x] **CSV Report Export**:
  - Direct export of all duplicate groups and paths into spreadsheet-compatible format.
- [x] **Configuration Persistence**:
  - Settings stored on flash drive at `/boot/config/plugins/dedupe/dedupe.cfg`.

