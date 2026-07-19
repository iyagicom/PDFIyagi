# AI7 (.ai7 / .iyagi) 포맷 명세서 — v1.2 (Current) / v1.0 (LTS)

> AI 시대의 지능형 문서 표준 — 기획안 「AI-Native 차세대 문서 포맷 .iyagi」의 구현 명세.
> 이 문서는 **실제 구현과 1:1로 일치**하는 것을 원칙으로 하며, 구현이 바뀌면 명세를 함께 갱신한다.
> 외부 개발자/AI용 요약은 [`AI7_QUICKSTART.md`](AI7_QUICKSTART.md) 참조.

## 0. 스펙 버전과 프로파일 (2026-07-19 확정)

PDF가 1.4/1.7/PDF-A를 병행 유지하듯, AI7도 안정판과 최신판을 병행한다:

| 프로파일 | 스펙 버전 | **Security Profile** | 상태 | 내용 |
|---------|----------|----------------------|------|------|
| **LTS** | **v1.0** | **Strict** | Stable — 장기 지원, 보안 수정만 | 최대 호환·검증 완료. 실행/외부유입 능동 요소 없음(래스터/텍스트/document.md/오버레이/임베딩/이력/지식그래프 — 전부 수동·파생 데이터) |
| **Current** | **v1.2** | **Extended** | 최신 | v1.0 + `comments/` 코멘트 레이어 + history `actor` 기록 |

**Security Profile (2026-07-19, GPT 검토 반영)** — 정책 수립자가 쓰기 쉽게 라벨화:

| Security Profile | 허용 | 배제 | 대상 |
|------------------|------|------|------|
| **Strict** | pages/·content/·annotations/·embeddings/·history/·ai/ (전부 수동·파생 데이터) | `comments/`(외부 텍스트 유입), `actor`, `agent_scripts/`(실행) | 관공서·기업 — "AI7 v1.0 Strict만 허용" 정책 |
| **Extended** | 위 전부 + `comments/` + `actor` | `agent_scripts/`(모든 프로파일에서 여전히 보류) | 최신 협업 기능 사용 |

- 판정 기준은 **"능동성·외부 유입"** 이다 — 실행 요소(agent_scripts)와 외부가 심는
  텍스트(comments)가 진짜 공격면이다. 임베딩/이력은 파생·수동 데이터라 크기 상한(§6.1)만
  걸면 안전하므로 Strict에도 포함한다(검색·되돌리기 기능 유지).
- `metadata.json`의 `securityProfile` 필드("strict" | "extended")가 이 파일의 보안
  프로파일을 선언한다. **빈 값이면 strict로 간주**한다(보수적 기본값).
- `metadata.json`의 `specVersion` 필드("1.0" | "1.2")가 이 파일의 기능 버전을 선언한다
  (없으면 v1.0으로 간주 — v1.2 확장 디렉토리가 있어도 리더는 v1.0 부분만 읽으면 된다).
- 신기능이 늘수록 파서·JSON·디렉토리가 늘어 공격면(attack surface)도 넓어진다 —
  보수적인 환경(관공서·기업)은 **v1.0 Strict만 허용**하는 정책을 쓸 수 있다.
- v1.0 리더가 v1.2 파일을 열어도 안전하다 — §6 원칙(모르는 디렉토리/키 무시)에 따라
  comments/ 등을 건너뛰면 v1.0과 동일하게 동작한다.
- 이 저장소의 보안 수정(마킹-이력 충돌 해소, 압축폭탄 픽셀 상한)은 **두 프로파일 공통**이다.
- **PDFIyagi는 두 프로파일 저장을 모두 지원한다** — 파일 메뉴 "ai7 저장"(v1.2) /
  "ai7 저장 (v1.0 호환)"(comments 유실 경고 후 저장). 리더는 두 프로파일 모두 읽는다.

## 1. 비전과 위치

AI7은 개인 편집 도구의 저장 포맷이 아니라 **배포용 AI-native 문서 표준(PDF의 후계)** 을
목표로 한다. 기존 PDF/스캔 문서를 **원본 그대로 품고 들어와서**(래스터 레이어), 그 위에
**의미 구조를 단계적으로 입히는**(시맨틱 레이어) 마이그레이션 경로를 제공한다.

- 내용(content) / 구조(structure) / 표현(representation) 분리
- 어디서 열어도 똑같이 보임 (래스터 레이어가 보장)
- AI가 파싱 없이 바로 읽는 구조화 콘텐츠 (시맨틱 레이어가 보장)

**Seven Capabilities**: Read(L0/L1) → Understand(document.md) → Structure(표/이미지 메타)
→ Connect(document.kg) → Search(embeddings/) → Reason(조합+LLM) → Act(오버레이+history/).
AI7은 문서를 저장하는 파일이 아니라 AI가 활용할 수 있는 지식으로 변환하는 문서 아키텍처다.

## 2. 컨테이너

- **ZIP 아카이브** (압축 방식: deflate, Qt QZipWriter/QZipReader 호환)
- 확장자: `.ai7` (장기적으로 `.iyagi` 병행)
- 모든 JSON은 UTF-8, 모든 텍스트 파일은 UTF-8

### 2.1 파일 트리 (v1.2 기준)

```
metadata.json                  문서 메타데이터 (필수)
manifest.json                  전체 파일 목록 + 스키마 버전 (필수)
pages/page_NNNN.webp           Layer 0: 페이지 래스터 (필수, 페이지당 1개)
content/page_NNNN.json         Layer 1: 텍스트 블록 (OCR 또는 원문 추출)
content/document.md            Layer 1.5: 시맨틱 마크다운 (문서 전체)
annotations/page_NNNN.json     Layer 2: 편집 오버레이 (있는 페이지만)
resources/clip_pNNNN_K.png     오버레이가 참조하는 이미지 리소스
comments/page_NNNN.json        코멘트·피드백 — 페이지 귀속 (v1.2 신설)
comments/document.json         코멘트·피드백 — 문서 전체 귀속 (v1.2 신설)
```

`NNNN`은 0-패딩 4자리 페이지 인덱스(0부터).

### 2.2 예약 디렉토리 (기획안 로드맵, 아직 미기록)

| 디렉토리 | 용도 | 상태 |
|---------|------|------|
| `embeddings/` | 로컬 벡터 임베딩 (RAG) | **v1.0 구현** |
| `history/` | 버전 관리 (편집 이력) | **v1.0 구현** |
| `ai/` | AI 보조 데이터 (`document.kg` 지식그래프) | **v1.0 구현** |
| `agent_scripts/` | 문서 내 에이전트 스크립트 | **보류** (보안 검토 전) |

리더는 모르는 디렉토리/파일을 **반드시 무시**해야 한다 (§6 호환성).

## 3. 공통 JSON 헤더

모든 JSON 파일은 다음 두 필드로 시작한다:

```json
{ "_format": "ai7-metadata", "_version": "1.0", ... }
```

- `_format`: 파일 종류 식별자 (`ai7-metadata`, `ai7-content`, `ai7-annotation`, `ai7-manifest`)
- `_version`: 그 파일 스키마의 버전. `major.minor` — major가 다르면 리더는 해석 포기 가능,
  minor 증가는 하위 호환(필드 추가만) 보장.
- 모든 스키마는 `extensions`(서드파티 확장)와 `_reserved`(포맷 자체 확장) 객체를 갖는다.
  리더는 모르는 키를 무시하고, 라이터는 읽은 값을 보존해 다시 쓰는 것을 권장.

## 4. 좌표계

- 기준 단위: **PDF pt** (1/72 inch). 페이지 좌상단 원점, y 아래 방향.
- 페이지 크기: `metadata.json`의 `document.defaultWidth/Height`(pt) 또는 페이지별 값.
- 래스터와의 관계: `content/page_NNNN.json`의 `ocr.sourceDpi`가 그 페이지 래스터의
  DPI. `px = pt / 72 × sourceDpi`.
- 텍스트 블록 `rect`는 pt. 오버레이의 `ptToPxAtSave`는 저장 시점 표시 배율(복원용).

## 5. 레이어 정의

### Layer 0 — 래스터 (`pages/*.webp`)
- WEBP. **항상 무손실(quality 100)** (2026-07-09부터 — 이전엔 4천만px 초과 시
  quality 95로 낮췄으나, 고DPI 스캔에서 PDF 저장 경로의 "100% 그대로 저장"
  기본값과 화질 차이가 나던 것을 맞춤. 병렬 인코딩+이벤트 펌핑으로 큰 페이지도
  UI가 멎지 않으므로 시간 부담은 감수).
- WEBP 규격 한계(한 변 16383px) 초과 이미지는 한계 내로 축소 후 기록.
- 이 레이어만 있으면 문서는 "보이는" 상태로 완전하다 — 모든 상위 레이어는 선택적.

### Layer 1 — 텍스트 블록 (`content/page_NNNN.json`)
```json
{ "_format": "ai7-content", "_version": "1.1", "pageIndex": 0,
  "pageImage": { "width": 3297, "height": 4666, "dpi": 400,
                  "encoding": "webp-lossless|webp-q95", "sha256": "..." },
  "ocr": { "engine": "pp-ocrv5|tesseract|null(원문)", "language": "korean",
            "processedAt": "ISO8601", "sourceDpi": 400 },
  "blocks": [ { "id": "blk_0000", "type": "word|line|block",
                 "text": "...", "rect": [x,y,w,h], "confidence": 0.0,
                 "props": {} } ] }
```
- PDF에서 변환된 문서는 OCR 대신 **원문 텍스트 레이어**를 저장(confidence 1.0).
- 블록 순서 = 읽기 순서 (위→아래, 왼→오른쪽).

### Layer 1.5 — 시맨틱 마크다운 (`content/document.md`)
- 문서 전체를 하나의 Markdown으로 — **AI/RAG가 파싱 없이 바로 소비하는 파일**.
- 페이지 경계는 `<!-- page N -->` 주석으로 표시.
- 제목 추론: 블록 높이가 본문 중앙값의 1.9배 초과 → `#`, 1.4배 초과 → `##`.
- 문단 구분: 줄 간격이 줄 높이의 1.4배 초과 → 빈 줄.
- 라이터는 저장 시마다 Layer 1에서 재생성한다 (파생 데이터 — 원본은 Layer 1).

### Layer 2 — 편집 오버레이 (`annotations/page_NNNN.json`)
```json
{ "_format": "ai7-annotation", "_version": "1.0", "pageIndex": 0,
  "items": [
    { "id": "ann_0001", "type": "redact",   "subtype": "blackbox|blur",
      "rect": [..], "color": "#AARRGGBB", "blurRadius": 0, "props": {} },
    { "id": "ann_0002", "type": "textEdit", "blockIdx": 3,
      "blockPdfRect": [..], "originalPdfRect": [..], "ptToPxAtSave": 2.08,
      "fontFamily": "...", "fontSize": 13.5, "bold": false, ... },
    { "id": "ann_0003", "type": "clipImage", "imageFile": "resources/clip_p0000_0.png",
      "pos": [x,y], "itemRect": [..], "pdfRect": [..], "bakedPdfRect": [..],
      "imageMeta": { "width": 3297, "height": 4666, "impliedDpi": 400,
                      "encoding": "png", "sha256": "..." } } ] }
```

**이미지 메타데이터**: 모든 이미지(페이지·리소스)는 자동 파생 메타
(크기/DPI/인코딩/sha256 — 무결성·중복 제거)를 갖는다. 다음 필드는 AI 연동 후
채워지는 **예약 필드**로, 라이터는 값이 있으면 보존해야 한다:
`description`(자연어 설명), `objects[]`(감지된 객체), `captionEmbedding`(캡션 벡터).
- 오버레이는 **비파괴** — Layer 0을 수정하지 않고 위에 얹는다. 다시 열면 계속 편집 가능.

### Layer 3 — 벡터 임베딩 (`embeddings/`)
```json
// embeddings/index.json
{ "_format": "ai7-embeddings", "_version": "1.0",
  "model": "distiluse-base-multilingual-cased-v2-int8",
  "dim": 768, "normalized": true, "count": 42,
  "vectorFile": "embeddings/vectors.bin",
  "chunks": [ { "id": "chk_0000", "page": 0, "text": "..." } ] }
```
- `vectors.bin`: float32 little-endian, row-major — i번째 청크의 벡터는
  `[i*dim, (i+1)*dim)`. 벡터는 L2 정규화돼 있어 **코사인 유사도 = 내적**.
- 청크: 문단 단위(~250자), 읽기 순서. URL/쪽번호 등 노이즈는 제외.
- 파생 데이터 — 라이터는 저장 시 Layer 1에서 재생성한다.
- RAG 소비 방법: index.json의 model로 질의를 임베딩 → vectors.bin과 내적 →
  상위 청크의 text/page 사용. 모델이 다르면 청크 text만 쓰고 재임베딩.

### Layer 4 — 편집 이력 (`history/`)
```json
// history/rev_NNNN.json — 그 시점의 편집 상태 스냅샷
{ "_format": "ai7-history-rev", "_version": "1.1",
  "savedAt": "ISO8601", "software": "PDFIyagi/1.25.4",
  "actor": { "type": "human | agent", "name": "표시명" },   // v1.2 — 그 저장의 행위자
  "annotations": { "annotations/page_0000.json": { ...당시 전체 내용... } } }
// history/log.json — 리비전 목록 (최신 라이터가 재생성)
{ "_format": "ai7-history-log", "_version": "1.0",
  "revisions": [ { "rev": "history/rev_0000.json", "savedAt": "...", "software": "...",
                   "actor": { "type": "...", "name": "..." } } ] }
```
- **actor(v1.2)**: `metadata.json`의 `actor`(이번 저장의 행위자)가 다음 저장 때
  "직전 상태" 리비전으로 이월되어 "누가 저장했었는지"가 이력에 남는다. 서명 없는
  자기신고 값 — **감사 추적이 아니라 참고 정보**다. 구버전 리비전은 null.
- 저장할 때마다 **직전 파일의 어노테이션(편집 상태)** 이 새 리비전으로 보존된다 —
  어노테이션은 KB 단위라 저렴하고, 편집 되돌리기의 원료가 된다.
- 기존 history/는 그대로 이월(§6 보존 원칙). **최근 20개 리비전만 유지.**
- **정보삭제 마킹과의 충돌 해소(v1.2 보안)**: 이번 저장에 마킹(blackbox/blur)이
  **새로 추가**되면 이월 이력과 직전 스냅샷을 전부 버린다 — 이력에 남은 마킹 이전
  상태가 마킹을 무력화하는 것을 막기 위함. 마킹 개수가 그대로인 재저장부터는 정상적으로
  이력이 다시 쌓인다(마킹 이후 상태는 안전). 같은 이유로 라이터는 마킹 사각형과 겹치는
  Layer 1 텍스트 블록을 기록에서 제외해야 한다(document.md/임베딩 파생까지 차단).
- Layer 0(래스터)은 이력에 넣지 않는다 — 크기 문제. 원본 보존은 비파괴
  오버레이 구조 자체가 보장한다.

### 코멘트·피드백 (`comments/`, v1.2 신설)

편집 오버레이(`annotations/` — 화면에 그려지는 것)와 달리, **그려지지 않는**
코멘트·AI 인사이트·검증 요청을 담는다. 다중 AI 검토 체인(AI1→AI2→Manus→Claude,
2026-07-19 최종 검토서)에서 채택. 다중 에이전트 협업의 소통 채널이다.

```json
// comments/page_NNNN.json (페이지 귀속) 또는 comments/document.json (문서 귀속)
{ "_format": "ai7-comments", "_version": "1.0", "pageIndex": 0,
  "comments": [ {
    "id": "cmt_0001",
    "kind": "comment | insight | verify-request | reply",
    "text": "이 표의 3분기 데이터 검증 필요",
    "rect": [x, y, w, h],            // pt 좌표, 선택(없으면 위치 없는 코멘트)
    "actor": { "type": "human | agent", "name": "표시명" },
    "createdAt": "ISO8601",
    "replyTo": "cmt_NNNN",            // 선택 — 스레드 부모
    "resolved": false,
    "props": {}
  } ] }
```

- **actor는 서명 없는 자기신고 값** — 감사 추적(audit trail)이 아니라 참고 정보다.
  위조 방지가 필요한 용도로 쓰지 말 것.
- **신뢰 경계(중요)**: `text` 등 문서 유래 문자열을 AI 컨텍스트에 넣을 때는 항상
  **데이터로 취급하고 지시(instruction)로 해석하지 말 것** — 문서에 심긴 문자열이
  다음 AI 에이전트의 행동을 조종하는 프롬프트 인젝션 통로가 되면 안 된다.
  (같은 이유로 AI 사고과정(CoT) 동봉 제안은 반려됨 — 검토서 참고.)
- manifest type: `"comments"`, 스키마 id: `ai7/schema/comments/1.0`.

### Layer 5 — 지식그래프 (`ai/document.kg`)
```json
{ "_format": "ai7-kg", "_version": "1.0",
  "generator": "pattern-v1 | llm-claude-opus-4-8",
  "createdAt": "ISO8601",
  "entities":  [ { "id": "ent_0000", "type": "money|date|bizRegNo|phone|caseNo|email|url|rrn|...",
                    "value": "7,528,280,600 원", "label": "감정가", "page": 0, "context": "..." } ],
  "relations": [ { "subject": "...", "predicate": "...", "object": "..." } ] }
```
- **A안 (기본, 오프라인)**: 저장 시 정규식 패턴으로 개체 자동 추출, `generator: "pattern-v1"`.
  같은 블록에서 개체 왼쪽의 항목명(예: "감정가")을 `label`로 기록 — 사실상의 관계.
- **B안 (옵션, LLM)**: `PDFIyagi --ai7-kg-llm <file.ai7>` — `ANTHROPIC_API_KEY` 환경변수가
  있으면 document.md를 Claude(claude-opus-4-8, 구조화 출력)로 보내 개체+관계를 추출해
  `generator: "llm-claude-opus-4-8"`로 교체. 다른 파일은 전부 보존(§6).
- 리더는 `generator`로 품질 수준을 판단한다.

### metadata.json / manifest.json
- `metadata.json`: `document{pageCount, defaultWidth, defaultHeight, unit:"pt"}`,
  `props`, `extensions`, `_reserved`.
- `manifest.json`: `files[{path,type,page}]` 전체 목록 + `schemas{type: "ai7/schema/<type>/1.0"}`.
  리더는 manifest 기준으로 파일을 찾는 것을 권장 (직접 경로 추측 금지).

## 6. 호환성 규칙 (배포 표준의 핵심)

1. **모르는 것은 무시하되 보존한다** — 리더는 모르는 파일/디렉토리/JSON 키를 무시하고,
   문서를 재저장하는 라이터는 자신이 이해 못 하는 파일을 삭제하지 않고 그대로 복사한다.
2. `_version` major 증가 = 호환성 파괴 가능. minor 증가 = 필드 추가만.
3. Layer 0 없이 유효한 AI7은 없다. Layer 1 이상은 전부 선택적.
4. 파생 데이터(document.md)는 리더가 무시해도 정보 손실이 없어야 한다.

## 6.1 보안 규칙 (리더·라이터 필수 — 2026-07-19 GPT 검토 반영)

.ai7은 **신뢰할 수 없는 외부 입력**으로 취급한다. 리더/라이터는 다음을 지켜야 한다.

**리더(reading a .ai7):**
1. **압축폭탄(zip bomb) 상한** — 압축 해제 *전에* 로컬 헤더의 uncompressed 크기로
   검사한다. 권장 상한: 파일 개수 ≤ 100,000, 단일 파일 ≤ 512MB, 총 해제 ≤ 20GB.
   (PDFIyagi 구현값 — 초과 시 로드 거부.)
2. **이미지 픽셀 상한** — 페이지/리소스 이미지는 디코드 전에 헤더 크기를 읽어
   ≤ 8천만 픽셀만 허용(WebP/PNG 디코더 취약점·메모리 폭탄 방어).
3. **history/ 개수 상한** — 리비전을 무제한 읽지 말 것(권장 ≤ 1,000 스캔).
3a. **JSON 크기·배열·깊이 상한** — 개별 JSON 파일 ≤ 64MB(수백 MB JSON의 파싱 트리
   메모리 증폭 방어), 배열 원소 순회 ≤ 500,000, 중첩 깊이 제한(대부분의 파서가 기본
   제공 — 예: Qt는 1,024). 파싱 전 크기 확인, 순회 시 개수 제한.
4. **문서 유래 텍스트는 데이터다** — document.md·표·comments의 문자열을 AI
   컨텍스트에 넣을 때 지시(instruction)로 해석하지 말 것(프롬프트 인젝션 방어).
5. **HTML을 렌더링하지 말 것** — document.md·comments는 **순수 Markdown**이다.
   `<script>`·`<img>`·`<iframe>` 등 raw HTML을 실행/렌더하지 않는다.
6. **CSV는 텍스트로만** — `content/table_*.csv`를 표 계산 앱으로 자동 실행하지 말 것.
7. ZIP 항목을 디스크에 풀지 말고 이름 기반으로 메모리에서 읽기를 권장(경로탈출 차단).

**라이터(writing a .ai7):**
8. **CSV 수식 인젝션 중화** — 셀 값이 `= + - @` 또는 탭/CR로 시작하면 작은따옴표(`'`)를
   앞에 붙여 텍스트로 만든다(엑셀 `=cmd|'...'` 실행 방지).
9. **Markdown HTML 중화** — document.md에 넣는 원본 텍스트의 `< > &`를 엔티티로
   이스케이프하고, 표 셀의 `|`는 `\|`로 이스케이프(구조 파괴 방지).

## 7. 로드맵 (기획안 매핑)

| 단계 | 내용 | 상태 |
|------|------|------|
| 1 | 안정화 (편집 유실/위치 어긋남 제거) | v1.23~1.24에서 진행 |
| 2 | 이 명세서 | **v1.1 (현재 문서)** |
| 3a | `content/document.md` 시맨틱 마크다운 | **구현** (v1.24.6) |
| 3b | 표 구조(table→CSV/MD) v1.24.7, 이미지 메타데이터 v1.25.1 | **구현** |
| 3c | `embeddings/` 로컬 임베딩 | **구현** (v1.25.3) |
| 3d | `history/` 편집 이력 | **구현** (v1.25.4) |
| 3e | `ai/document.kg` 지식그래프 | **구현** (v1.25.5 — A안 패턴 + B안 LLM 옵션) |
| — | `agent_scripts/` | 보류 (보안) |
