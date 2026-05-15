# SIM-APDU-Analyzer User Guide v1.0.3

---

## Table of Contents

1. [Starting the Application](#1-starting-the-application)
2. [Loading a Log](#2-loading-a-log)
3. [Running Analysis](#3-running-analysis)
4. [APDU Tab — Summary Panel](#4-apdu-tab--summary-panel-left)
5. [APDU Tab — Application Layer](#5-apdu-tab--application-layer-right-top)
6. [APDU Tab — Transport Layer](#6-apdu-tab--transport-layer-right-bottom)
7. [File System Tab — File System Tree](#7-file-system-tab--file-system-tree-left)
8. [File System Tab — File Contents](#8-file-system-tab--file-contents-right)

---

## 1. Starting the Application

1. Double-click `SIM-APDU-Analyzer.exe`
2. A console window opens and the server starts
3. Your browser opens automatically
4. The page shows a purple header with "🔍 SIM APDU Analyzer v1.0.0"

> Keep the console window open while using the tool. Closing it stops the server.  
> If the browser doesn't open, navigate to http://127.0.0.1:8090 manually.

---

## 2. Loading a Log

The control panel at the top has two input buttons.

### 2.1 Upload a File

1. Click **📁 Open Log File**
2. Select a `.txt` file containing modem diagnostic logs
3. The status bar shows the filename and line count (e.g., `✓ log.txt (2352 lines)`)

### 2.2 Paste from Clipboard

1. In your diagnostic tool (QXDM, ELT, etc.), select all and copy (Ctrl+A → Ctrl+C)
2. Click **📋 Paste Log**
3. Allow clipboard permission when prompted
4. The status bar shows the result
5. Pasted logs are saved to `logs/_YYYYMMDD_HHMMSS.txt` next to the exe (folder created automatically)

### 2.3 Supported Formats and Capture Methods

| Format | Detection | Modem |
|--------|-----------|-------|
| QXDM / QCAT | Contains `[0x19B7]` | Qualcomm |
| Shannon DM | Contains `USIM_MAIN` | Samsung |
| ELT (MTK) | Contains `APDU_tx`, `MOD_SIM` | MediaTek |

**QXDM (Qualcomm):**
1. Apply log mask: `QXDM_log_mask_UIM_0x19B7.dmc` (included in Release)
2. Two filter views appear — select the one containing `0x19B7` logs
3. Select all: Ctrl+A → Copy: Ctrl+C → Use **📋 Paste Log**

**QCAT (Qualcomm):**
1. Open Qualcomm log file in QCAT (`.isf`, `.hdf`, `.bin`, etc.)
2. Filter by `19B7` to show UIM messages
3. File > Save As Text to export as `.txt`
4. Use **📁 Open Log File** to upload

**Shannon DM (Samsung):**
1. License: Elevated license required for UICC APDU access. Copy license file to Shannon DM install folder (overwrite existing)
2. CP bin matching: Open Preferences → select `Message - String - Trace` on the left → click Load → select the CP binary matching the device currently being logged (device version and build state must match exactly)
3. Trace tab: A `Trace` tab appears at the top of the main window
4. Filter: Set Filter selection to `USIM MAIN`, Keyword include to `UICC APDU`
5. Select all logs → save as `.txt` → upload

**ELT (MediaTek):**
1. View > PS Modules > SIM > MOD_SIM & MOD_SIM_2
2. Select all: Ctrl+A → Right-click → **Copy full messages**
3. Save as `.txt` and upload, or paste directly via **📋 Paste Log**

> Loading a new log replaces the previous session.

---

## 3. Running Analysis

1. Select **SIM1** or **SIM2** from the dropdown
2. Click **🔍 Analyze**
3. After 1–5 seconds, the status bar updates: `SIM1 | 475 commands | 100 files`
4. Two tabs appear: **APDU** and **File System**

> You can re-analyze with a different SIM port without reloading. Change the dropdown and click Analyze again.

---

## 4. APDU Tab — Summary Panel (Left)

All APDU commands listed in chronological order.

### 4.1 Table Columns

| Column | Content |
|--------|---------|
| # | APDU index number |
| Time | Modem timestamp (HH:MM:SS.mmm) |
| (icon) | Command type indicator |
| Command | APDU command name (SELECT, READ BINARY, ENVELOPE, etc.) |
| Detail | Target file name, SFI info, or CAT command type |

### 4.2 Color Coding

| Color | Meaning |
|-------|---------|
| Black | Successful command |
| Gray | Informational (INS echo, file not found) |
| Red | Error response |

### 4.3 Selecting a Command

- **Click** a row → highlights blue, right panel loads details
- **↑ / ↓ keys** → navigate between rows, right panel updates automatically

### 4.4 Searching

1. Type a keyword in the **Search** box (e.g., `SELECT`, `EPSLOCI`, `6FE3`)
2. **Enter** → jump to first match (orange highlight)
3. **Enter** or **▼** → next match
4. **▲** → previous match
5. Counter shows current/total (e.g., `3/15`)
6. **✕** → clear search

---

## 5. APDU Tab — Application Layer (Right, Top)

Shows decoded details of the selected command.

### 5.1 APDU Fields

| Field | Description |
|-------|-------------|
| CLA | Class byte — logical channel info |
| INS | Instruction — command name |
| P1 | Parameter 1 |
| P2 | Parameter 2 |
| Lc | Command data length |
| Le | Expected response length |
| Command Data | Data sent to the card |
| Response Data | Data returned by the card |
| Status Word | Result code (green=success, red=error, orange=warning) |

Original hex bytes are shown in small gray text below each field. These are useful for verifying decoded results against raw data. TLV-structured data is color-coded: tag (blue), length (orange), value (gray).

### 5.2 FCP (File Control Parameters)

Appears automatically below the APDU table for SELECT/STATUS commands:
- File Descriptor (file type, structure)
- File ID
- Security Attributes (access rules)
- File Size
- Life Cycle Status

### 5.3 CAT (Card Application Toolkit)

Appears for ENVELOPE, FETCH, TERMINAL RESPONSE commands:
- TLV fields decoded and displayed as a table (field name, hex, description)

### 5.4 🔍 Decoding Button

Appears (green) when additional decoding is available:
1. Click → decoded view appears below
2. Available for:
   - OTA Secured Data (decrypted OTA payload)
   - EF binary contents (pySim JSON decode)
   - Terminal Profile (capability bits interpretation)
   - Concatenated SMS: appears on the last part with all parts merged for full decoding

---

## 6. APDU Tab — Transport Layer (Right, Bottom)

Shows raw TX/RX byte sequences. **Collapsed by default.**

### 6.1 Expand / Collapse

- Click **"🔄 Transport Layer ▶"** header → expands (50/50 with Application Layer)
- Click **"🔄 Transport Layer ▼"** header → collapses to header only

### 6.2 Table Columns

| Column | Content |
|--------|---------|
| # | Sequence number within this exchange |
| Time | Timestamp |
| Dir | 📱 = Terminal→UICC (TX), 💳 = UICC→Terminal (RX) |
| Hex | Raw hex bytes |

Multiple protocol exchanges (ACK, INS echo, GET RESPONSE chain) are combined into a single APDU view.

### 6.3 📋 Copy Button

Copies the Transport table as tab-separated text (Time, Dir, Hex) to clipboard. Can be pasted directly into Excel.

---

## 7. File System Tab — File System Tree (Left)

Click the **File System** tab at the top to switch.

Shows all EFs read or updated during the session in a hierarchical tree.

### 7.1 Tree Structure

- **MF (3F00)** → DF.TELECOM, DF.GSM (collapsed by default)
- **ADF.USIM (AID)** → EF list and sub-DFs (expanded by default)
- **ADF.ISIM (AID)** → EF list (expanded by default)

### 7.2 Controls

- Click **▼/▶** next to a DF → toggle that section
- **▼ Expand All** → show everything
- **▶ Collapse All** → hide all sub-DFs
- **Search** box → filter by File Name or FID

### 7.3 Table Columns

| Column | Description |
|--------|-------------|
| File Name | EF name (e.g., EF.IMSI). Unknown files show FID only |
| FID | File Identifier (hex) |
| Type | TF = Transparent File, LF = Linear Fixed File |
| ARR | Access Rule Reference number |
| REC# | Record number (LF only, `-` for TF) |
| OFS | Byte offset (TF only) |
| LEN | Data length (decimal) |
| REF | APDU index — **clickable link** |

### 7.4 Yellow Rows (OTA Updates)

- Yellow background = file value **changed during the session**
- Linear Fixed (LF): detected when the same record number has different contents
- Transparent (TF): compares **overlapping byte regions** between different READ/UPDATE operations on the same file (detects changes even when offsets differ, as long as byte ranges overlap)
- Both the original (READ) and updated (UPDATE) values shown as separate rows
- Click the REF link to see the exact READ/UPDATE command details

### 7.5 Clicking a REF Link

1. Click the blue REF number (e.g., `74`)
2. Switches to APDU tab and scrolls to that command

### 7.6 Clicking an EF Row

1. Click any EF row (not the REF link)
2. Right panel (File Contents) shows that EF's data

---

## 8. File System Tab — File Contents (Right)

### 8.1 Decode / Raw Toggle

Toggle buttons at the top:
- **Decode** (default): Structured decoded view. Shows "No decoded data" for unsupported EFs
- **Raw**: Original hex string

Decode mode provides the following specialized views depending on the EF type:

#### PLMN Table

Applies to: OPLMNwAcT, PLMNwAcT, HPLMNwAcT, RPLMNAcTD, FPLMN, EHPLMN, PLMNsel, UPLMNWLAN, OPLMNWLAN, WLRPLMN

| Column | Description |
|--------|-------------|
| # | Entry number within the file |
| MCC | Mobile Country Code |
| MNC | Mobile Network Code |
| AcT | Hex value + decoded names in parentheses (e.g., `C0C0 (UTRAN, E-UTRAN, GSM, GSM COMPACT)`) |

- Empty entries (FFFFFF) show `-`
- For files exceeding 256 bytes, boundary entries show `…` for MCC/MNC, with AcT decoded when positionally available
- `#` reflects actual position within the file (sequential across multi-chunk reads)

#### Service Table (UST / IST / EST)

Applies to: EF.UST, EF.IST, EF.EST

| Column | Description |
|--------|-------------|
| # | Service number |
| Service | Service name (per TS 31.102) |
| Status | ON (green) / OFF (gray) |

#### ACC Table

Applies to: EF.ACC

| Column | Description |
|--------|-------------|
| # | Bit number |
| Class | Access Control Class (Class 0–15) |
| Status | ON / OFF |

#### ARR Table

Applies to: EF.ARR (Access Rule Reference)

| Column | Description |
|--------|-------------|
| # | Record number |
| Read | Read access condition |
| Update | Update access condition |
| Write | Write access condition |
| Activate | Activate condition |
| Deactivate | Deactivate condition |

#### URSP Rule Tree

Applies to: EF.URSP (4F0B)

Displays URSP rules in a hierarchical tree format. Traffic Descriptors and Route Selection Descriptors are shown with indentation.

#### General JSON View

EFs not covered by the above views are decoded via pySim and displayed as JSON (160+ EFs supported).

### 8.2 📋 Copy

- Copies current view to clipboard
- Raw mode: copies original hex string as-is
- Decode mode: tables copied as tab-separated (Excel-ready), text copied as-is

---

**© 2026 JUSEOK AHN <ajs3013@lguplus.co.kr> All rights reserved.**
