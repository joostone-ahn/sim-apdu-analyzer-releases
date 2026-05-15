# SIM-APDU-Analyzer 사용자 가이드 v1.0.3

---

## 목차

1. [프로그램 실행](#1-프로그램-실행)
2. [로그 불러오기](#2-로그-불러오기)
3. [분석 실행](#3-분석-실행)
4. [APDU 탭 — Summary 패널](#4-apdu-탭--summary-패널-좌측)
5. [APDU 탭 — Application Layer](#5-apdu-탭--application-layer-우측-상단)
6. [APDU 탭 — Transport Layer](#6-apdu-탭--transport-layer-우측-하단)
7. [File System 탭 — 파일 시스템 트리](#7-file-system-탭--파일-시스템-트리-좌측)
8. [File System 탭 — File Contents](#8-file-system-탭--file-contents-우측)

---

## 1. 프로그램 실행

1. `SIM-APDU-Analyzer.exe` 파일을 더블클릭합니다
2. 콘솔 창이 열리며 서버가 시작됩니다
3. 브라우저가 자동으로 열립니다
4. 화면 상단에 보라색 헤더와 "🔍 SIM APDU Analyzer v1.0.0"이 표시됩니다

> 콘솔 창을 닫으면 프로그램이 종료됩니다. 사용 중에는 열어두세요.  
> 브라우저가 자동으로 열리지 않으면 http://127.0.0.1:8090 으로 접속하세요.

---

## 2. 로그 불러오기

상단 컨트롤 패널에 두 개의 버튼이 있습니다.

### 2.1 파일 업로드

1. **📁 Open Log File** 버튼을 클릭합니다
2. 파일 선택 창에서 모뎀 진단 로그 `.txt` 파일을 선택합니다
3. 상태 표시줄에 파일명과 라인 수가 표시됩니다 (예: `✓ log.txt (2352 lines)`)

### 2.2 클립보드 붙여넣기

1. 진단 도구(QXDM, ELT 등)에서 로그를 전체 선택 후 복사합니다 (Ctrl+A → Ctrl+C)
2. **📋 Paste Log** 버튼을 클릭합니다
3. 브라우저가 클립보드 권한을 요청하면 "허용"을 클릭합니다
4. 상태 표시줄에 결과가 표시됩니다
5. 붙여넣은 로그는 exe 옆 `logs/` 폴더에 `_YYYYMMDD_HHMMSS.txt` 이름으로 자동 저장됩니다 (폴더가 없으면 자동 생성)

### 2.3 지원 포맷 및 캡처 방법

| 포맷 | 감지 조건 | 모뎀 |
|------|----------|------|
| QXDM / QCAT | `[0x19B7]` 포함 | Qualcomm |
| Shannon DM | `USIM_MAIN` 포함 | Samsung |
| ELT (MTK) | `APDU_tx`, `MOD_SIM` 포함 | MediaTek |

**QXDM (Qualcomm):**
1. 로그 마스크 적용: `QXDM_log_mask_UIM_0x19B7.dmc` (Release에 포함)
2. 두 개의 필터 뷰가 나타남 — `0x19B7` 로그가 포함된 뷰 선택
3. 전체 선택: Ctrl+A → 복사: Ctrl+C → **📋 Paste Log** 사용

**QCAT (Qualcomm):**
1. QCAT에서 Qualcomm 로그 파일 열기 (`.isf`, `.hdf`, `.bin` 등)
2. 필터에서 `19B7` 검색하여 UIM 메시지 필터링
3. File > Save As Text로 `.txt` 저장
4. **📁 Open Log File**로 업로드

**Shannon DM (Samsung):**
1. 라이센스: UICC APDU 확인을 위해 권한 상승된 라이센스 필요. 라이센스 파일을 Shannon DM 실행 폴더에 덮어쓰기
2. CP bin 매칭: 상단 메뉴 Preference 클릭 → 좌측 `Message - String - Trace` 선택 → Load 버튼 클릭 → 현재 로깅 중인 단말의 CP binary 선택 (단말 버전 및 빌드 상태가 정확히 일치해야 정상 출력)
3. Trace 탭 확인: 메인 화면 상단에 `Trace` 탭이 생성됨
4. Filter 설정: Filter selection에서 `USIM MAIN` 선택, Keyword include에 `UICC APDU` 입력
5. 로그 전체 선택 → `.txt` 저장 → 업로드

**ELT (MediaTek):**
1. View > PS Modules > SIM > MOD_SIM & MOD_SIM_2 선택
2. 전체 선택: Ctrl+A → 우클릭 → **Copy full messages**
3. `.txt`로 저장 후 업로드 또는 **📋 Paste Log**로 직접 붙여넣기

> 새 로그를 불러오면 이전 분석 결과는 초기화됩니다.

---

## 3. 분석 실행

1. 우측 드롭다운에서 **SIM1** 또는 **SIM2**를 선택합니다
2. **🔍 Analyze** 버튼을 클릭합니다
3. 1~5초 후 상태 표시줄이 업데이트됩니다: `SIM1 | 475 commands | 100 files`
4. 하단에 **APDU** 탭과 **File System** 탭이 나타납니다

> 같은 로그에서 SIM 포트만 바꿔서 재분석할 수 있습니다. 드롭다운 변경 후 Analyze를 다시 클릭하세요.

---

## 4. APDU 탭 — Summary 패널 (좌측)

모든 APDU 명령이 시간순으로 나열됩니다.

### 4.1 테이블 구성

| 열 | 내용 |
|----|------|
| # | APDU 순서 번호 |
| Time | 모뎀 타임스탬프 (HH:MM:SS.mmm) |
| (아이콘) | 명령 유형 표시 |
| Command | APDU 명령 이름 (SELECT, READ BINARY, ENVELOPE 등) |
| Detail | 대상 파일명, SFI 정보, CAT 명령 유형 |

### 4.2 색상 구분

| 색상 | 의미 |
|------|------|
| 검정 | 정상 명령 |
| 회색 | 정보성 (INS echo, 파일 미발견) |
| 빨강 | 에러 응답 |

### 4.3 명령 선택

- 행을 **클릭**하면 파란색으로 강조되고, 우측 패널에 상세 정보가 로드됩니다
- **↑ / ↓ 키**로 행 간 이동이 가능하며, 우측 패널이 자동 업데이트됩니다

### 4.4 검색

1. 헤더의 **Search** 입력란에 키워드를 입력합니다 (예: `SELECT`, `EPSLOCI`, `6FE3`)
2. **Enter** → 첫 번째 매치로 이동 (주황색 강조)
3. **Enter** 또는 **▼** → 다음 매치
4. **▲** → 이전 매치
5. 카운터에 현재/전체 표시 (예: `3/15`)
6. **✕** → 검색 초기화

---

## 5. APDU 탭 — Application Layer (우측 상단)

선택한 명령의 디코딩 결과를 표시합니다.

### 5.1 APDU 필드

| 필드 | 설명 |
|------|------|
| CLA | Class 바이트 — 논리 채널 정보 |
| INS | Instruction — 명령 이름 |
| P1 | Parameter 1 |
| P2 | Parameter 2 |
| Lc | Command Data 길이 |
| Le | 예상 응답 길이 |
| Command Data | 카드로 전송한 데이터 |
| Response Data | 카드가 반환한 데이터 |
| Status Word | 결과 코드 (녹색=성공, 빨강=에러, 주황=경고) |

각 필드 아래에 원본 hex 바이트가 작은 회색 텍스트로 표시됩니다. 이 hex 값은 디코딩된 결과를 원본 데이터와 대조 검증할 때 유용합니다. TLV 구조의 데이터는 색상으로 구분됩니다: 태그(파란), 길이(주황), 값(회색).

### 5.2 FCP (File Control Parameters)

SELECT/STATUS 명령 시 APDU 테이블 아래에 자동 표시됩니다:
- File Descriptor (파일 유형, 구조)
- File ID
- Security Attributes (접근 규칙)
- File Size
- Life Cycle Status

### 5.3 CAT (Card Application Toolkit)

ENVELOPE, FETCH, TERMINAL RESPONSE 명령 시 표시됩니다:
- TLV 필드별 디코딩 결과를 테이블로 표시 (필드명, hex, 설명)

### 5.4 🔍 Decoding 버튼

추가 디코딩이 가능할 때 우측 상단에 녹색 버튼이 나타납니다:
1. 클릭하면 아래에 디코딩 결과가 표시됩니다
2. 대상:
   - OTA Secured Data (암호화된 OTA 페이로드 복호화)
   - EF 바이너리 내용 (pySim JSON 디코딩)
   - Terminal Profile (단말 기능 비트 해석)
   - Concatenated SMS: 분할 SMS의 마지막 파트에서 전체 데이터를 병합하여 디코딩 제공

---

## 6. APDU 탭 — Transport Layer (우측 하단)

원시 TX/RX 바이트 시퀀스를 표시합니다. **기본적으로 접혀 있습니다.**

### 6.1 펼치기 / 접기

- **"🔄 Transport Layer ▶"** 헤더를 클릭 → 펼침 (Application Layer와 50:50 공유)
- **"🔄 Transport Layer ▼"** 헤더를 클릭 → 접힘 (헤더만 표시)

### 6.2 테이블 구성

| 열 | 내용 |
|----|------|
| # | 교환 순서 번호 |
| Time | 타임스탬프 |
| Dir | 📱 = Terminal→UICC (TX), 💳 = UICC→Terminal (RX) |
| Hex | 원시 hex 바이트 |

여러 프로토콜 교환(ACK, INS echo, GET RESPONSE 체인)이 하나의 APDU 뷰로 결합됩니다.

### 6.3 📋 Copy 버튼

클릭하면 Transport 테이블이 탭 구분 텍스트(Time, Dir, Hex)로 클립보드에 복사됩니다. Excel에 바로 붙여넣기 가능합니다.

---

## 7. File System 탭 — 파일 시스템 트리 (좌측)

상단의 **File System** 탭을 클릭하여 전환합니다.

세션 중 READ 또는 UPDATE된 모든 EF를 계층 트리로 표시합니다.

### 7.1 트리 구조

- **MF (3F00)** → DF.TELECOM, DF.GSM (기본 접힘)
- **ADF.USIM (AID)** → EF 목록 및 하위 DF (기본 펼침)
- **ADF.ISIM (AID)** → EF 목록 (기본 펼침)

### 7.2 조작

- DF 행의 **▼/▶** 클릭 → 해당 섹션 펼치기/접기
- **▼ Expand All** → 전체 펼치기
- **▶ Collapse All** → 전체 접기
- **Search** 입력란 → File Name 또는 FID로 필터링

### 7.3 테이블 열

| 열 | 설명 |
|----|------|
| File Name | EF 이름 (예: EF.IMSI). 미확인 파일은 FID만 표시 |
| FID | File Identifier (hex) |
| Type | TF = Transparent File, LF = Linear Fixed File |
| ARR | Access Rule Reference 번호 |
| REC# | 레코드 번호 (LF만, TF는 `-`) |
| OFS | 바이트 오프셋 (TF만) |
| LEN | 데이터 길이 (10진수) |
| REF | APDU 인덱스 — **클릭 가능한 링크** |

### 7.4 노란색 행 (OTA 업데이트)

- 노란 배경 = 세션 중 파일 값이 **변경됨**
- Linear Fixed (LF): 같은 레코드 번호에 서로 다른 값이 기록된 경우 감지
- Transparent (TF): 같은 파일의 서로 다른 READ/UPDATE 간 **겹치는 바이트 영역**을 비교하여 값이 다르면 감지 (오프셋이 달라도 겹치는 구간이 있으면 비교)
- 변경 전(READ) 값과 변경 후(UPDATE) 값이 각각 별도 행으로 표시됩니다
- REF 링크를 클릭하면 해당 READ/UPDATE 명령의 상세를 확인할 수 있습니다

### 7.5 REF 링크 클릭

1. 파란색 REF 번호(예: `74`)를 클릭합니다
2. APDU 탭으로 전환되고 해당 명령으로 스크롤됩니다

### 7.6 EF 행 클릭

1. EF 행을 클릭합니다 (REF 링크가 아닌 행 자체)
2. 우측 패널(File Contents)에 해당 EF의 데이터가 표시됩니다

---

## 8. File System 탭 — File Contents (우측)

### 8.1 Decode / Raw 전환

상단에 **Decode**와 **Raw** 토글 버튼이 있습니다:
- **Decode** (기본): 구조화된 디코딩 뷰. 디코더가 없는 EF는 "No decoded data" 표시
- **Raw**: 원본 hex 문자열 표시

Decode 모드에서는 EF 유형에 따라 다음과 같은 전용 뷰가 제공됩니다:

#### PLMN 테이블

대상: OPLMNwAcT, PLMNwAcT, HPLMNwAcT, RPLMNAcTD, FPLMN, EHPLMN, PLMNsel, UPLMNWLAN, OPLMNWLAN, WLRPLMN

| 열 | 설명 |
|----|------|
| # | 파일 내 엔트리 순번 |
| MCC | Mobile Country Code |
| MNC | Mobile Network Code |
| AcT | hex 값 + 괄호 안에 기술명 (예: `C0C0 (UTRAN, E-UTRAN, GSM, GSM COMPACT)`) |

- 빈 엔트리(FFFFFF)는 `-`로 표시
- 256바이트 초과 파일의 경계 엔트리는 MCC/MNC가 `…`, AcT는 가능한 경우 정상 표시
- `#`은 파일 내 실제 위치를 반영 (2차 READ에서도 연속 번호 유지)

#### Service 테이블 (UST / IST / EST)

대상: EF.UST, EF.IST, EF.EST

| 열 | 설명 |
|----|------|
| # | 서비스 번호 |
| Service | 서비스 이름 (TS 31.102 기준) |
| Status | ON (녹색) / OFF (회색) |

#### ACC 테이블

대상: EF.ACC

| 열 | 설명 |
|----|------|
| # | 비트 번호 |
| Class | Access Control Class (Class 0~15) |
| Status | ON / OFF |

#### ARR 테이블

대상: EF.ARR (Access Rule Reference)

| 열 | 설명 |
|----|------|
| # | 레코드 번호 |
| Read | 읽기 접근 조건 |
| Update | 갱신 접근 조건 |
| Write | 쓰기 접근 조건 |
| Activate | 활성화 조건 |
| Deactivate | 비활성화 조건 |

#### URSP 규칙 트리

대상: EF.URSP (4F0B)

URSP 규칙을 계층 트리 형태로 표시합니다. 각 규칙의 Traffic Descriptor, Route Selection Descriptor가 들여쓰기로 구분됩니다.

#### 일반 JSON 뷰

위 전용 뷰에 해당하지 않는 EF는 pySim 기반 JSON 디코딩 결과를 표시합니다 (160+ EF 지원).

### 8.2 📋 Copy

- 클릭하면 현재 표시 중인 내용이 클립보드에 복사됩니다
- Raw 모드: 원본 hex 문자열 그대로 복사
- Decode 모드: 테이블은 탭 구분(Excel 붙여넣기 가능), 텍스트는 그대로 복사

---

**© 2026 JUSEOK AHN <ajs3013@lguplus.co.kr> All rights reserved.**
