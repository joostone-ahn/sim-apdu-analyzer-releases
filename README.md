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

**[Demo on Hugging Face Spaces](https://huggingface.co/spaces/Joostone/sim-apdu-analyzer)** — no installation required

### 💻 Run Locally

No Python installation required.

Download the latest exe from [Releases](../../releases) and run it directly.

---

## 📖 How to Use

1. Click **📁 Open Log File** or **📋 Paste Log** to load a modem diagnostic log
2. Select **SIM1** or **SIM2**, then click **🔍 Analyze**
3. Browse results in the **APDU** tab (command details) and **File System** tab (EF contents)

Supported formats: QXDM/QCAT (Qualcomm), Shannon DM (Samsung), ELT (MediaTek)

> For detailed instructions, see the User Guide PDF included in the release.

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
