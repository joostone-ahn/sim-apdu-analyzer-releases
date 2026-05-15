# 🔍 SIM-APDU-Analyzer

A web-based SIM/eSIM APDU log analysis tool.

[![Demo](https://img.shields.io/badge/🤗%20Demo-Hugging%20Face-yellow.svg)](https://huggingface.co/spaces/Joostone/sim-apdu-analyzer)
[![License](https://img.shields.io/badge/License-Proprietary-red.svg)](LICENSE)

---

## 💡 Why This Tool?

Traditional SIM tracers (e.g., COMPRION MiniMove) rely on physical contact interfaces and cannot intercept eSIM communication. This tool takes a different approach — parsing raw modem diagnostic traces to provide full visibility into both pSIM and eSIM APDU activity, without additional hardware. Supports Dual SIM (DSDS) by de-interleaving mixed APDU streams into distinct logical sessions.

---

## ✨ Key Features
- **No Hardware Required**: Parses internal modem diagnostic traces directly — no physical SIM tracer needed.
- **eSIM & Dual SIM**: De-interleaves mixed APDU traffic into independent SIM1/SIM2 sessions.
- **Layer Architecture**: Transport (raw TX/RX), Application (APDU decode with inline hex, FCP template parsing, CAT with hierarchical OTA structure).
- **EF Decoding**: 160+ Elementary Files decoded via pySim (JSON), with additional custom views for PLMN, service table, ACC, ARR.

---

## 🚀 Quick Start

### 🌐 Try Online

**[Demo on Hugging Face Spaces](https://huggingface.co/spaces/Joostone/sim-apdu-analyzer)**

### 💻 Download

Download the latest exe from [Releases](https://github.com/joostone-ahn/sim-apdu-analyzer-releases/releases).

---

## 📖 How to Use

See the User Guide for detailed instructions:
- [English](https://github.com/joostone-ahn/sim-apdu-analyzer-releases/blob/main/manual/user_guide_en_v1.0.0.md)
- [한국어](https://github.com/joostone-ahn/sim-apdu-analyzer-releases/blob/main/manual/user_guide_kr_v1.0.0.md)

---

## 📂 Supported Log Formats

Sample log files for each format are included in [Releases](https://github.com/joostone-ahn/sim-apdu-analyzer-releases/releases) as `sample_logs.zip`.

**QXDM / QXDM Pro** (Qualcomm)
```
[0x19B7]  07:18:00.539235  UIM APDU  1  SLOT_1 Type = TX Data = { 80 F2 00 0C 00  }
[0x19B7]  07:18:00.552341  UIM APDU  1  SLOT_1 Type = RX Data = { F2 84 10 A0 00 00 00 87 10 02 FF ... 90 00  }
```

**QCAT** (Qualcomm)
```
2026 Apr 14  00:02:24.427  [70]  0x19B7  UIM APDU
Subscription ID = 1
Slot Id = SLOT_1
Message Type = TX
TX Data = { 00 A4 00 04 02  }
```

**Shannon DM** (LSI Exynos)
```
14:31:20.231  14:31:29.450810 USIM_MAIN  DumpPrivacy  U0  [USIM_0] [UICC APDU CMD] Hex Dump -> : 00 A4 04 00 10 A0 00 ...
14:31:20.231  14:31:29.462041 USIM_MAIN  DumpPrivacy  U0  [USIM_0] [UICC APDU RSP] Hex Dump -> : A4 3F 00 62 33 82 02 ...
```

**ELT** (MediaTek)
```
6539, 0, 22416344, 19:46:43:980 2026/04/17, MOD_SIM, , MOD_SIM_BASELINE_UH, APDU_tx 0: 00 A4 00 04 02 3F 00 00
6568, 0, 22416957, 19:46:44:220 2026/04/17, MOD_SIM, , MOD_SIM_BASELINE_UH, APDU_rx 0: 62 33 82 02 78 21 83 02 3F 00 ...
```

---

## 📖 References

### 3GPP Standards
- [TS 31.102](https://www.3gpp.org/ftp/Specs/archive/31_series/31.102/) - USIM Application
- [TS 31.103](https://www.3gpp.org/ftp/Specs/archive/31_series/31.103/) - ISIM Application
- [TS 31.111](https://www.3gpp.org/ftp/Specs/archive/31_series/31.111/) - USIM Application Toolkit

### ETSI Standards
- [TS 101.220](https://www.etsi.org/deliver/etsi_ts/101200_101299/101220/) - UICC Framework (Tag assignments)
- [TS 102.221](https://www.etsi.org/deliver/etsi_ts/102200_102299/102221/) - UICC-Terminal Interface
- [TS 102.223](https://www.etsi.org/deliver/etsi_ts/102200_102299/102223/) - Card Application Toolkit
- [TS 102.225](https://www.etsi.org/deliver/etsi_ts/102200_102299/102225/) - Secured Packet Structure for UICC (OTA)

### Other Standards
- [ISO/IEC 7816-4](https://www.iso.org/standard/54550.html) - Integrated Circuit Cards
- [GSMA SGP.02](https://www.gsma.com/esim/) - Remote Provisioning Architecture

---

## 👤 Author

**JUSEOK AHN (안주석)**  
**Email**: ajs3013@lguplus.co.kr  
**Organization**: LG U+  
**Role**: Technical Specialist, Telecommunications Engineer

---

## 📄 License & Credits

**© 2026 JUSEOK AHN &lt;ajs3013@lguplus.co.kr&gt; All rights reserved.**

This software is proprietary and confidential. Developed for internal analysis, SIM validation, and automation of diagnostic workflows at LG U+.

EF decoding includes [pySim](https://github.com/osmocom/pysim) source code (GPL-2.0).

### Patent Information
This software is protected by patent applications filed with the Korean Intellectual Property Office.
