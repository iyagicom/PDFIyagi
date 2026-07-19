# AI7 File Format — Quick Start (for developers & AI agents)

> 전체 명세는 [`AI7FORMAT.md`](AI7FORMAT.md). 이 문서는 **SDK 없이** AI7을 읽는 데
> 필요한 최소 규칙만 담는다. 이 정도만 알면 대부분의 프로그램·AI가 바로 읽는다.
> (모든 .ai7 파일의 루트에는 같은 내용의 `README_AI.md`가 동봉된다.)

## 10가지 규칙 (AI7 v1.2)

1. **`.ai7`은 ZIP 컨테이너다.** 압축을 풀면 일반 파일들이다 (모든 텍스트는 UTF-8).
2. 압축을 풀면 **`manifest.json`을 먼저 읽는다.**
3. `manifest.json`에 **모든 파일의 역할(type)이 기록**되어 있다.
4. `metadata.json`에는 문서 정보(제목/작성일/소프트웨어/`specVersion`)가 있다.
5. `content/document.md`는 **AI가 읽을 본문**이다 (구조화된 Markdown).
6. `content/table_*.csv`는 표 데이터다.
7. `pages/*.webp`는 페이지 이미지다 (이것만으로 문서는 "보이는" 상태로 완전하다).
8. `annotations/`는 비파괴 편집 정보다 (원본 위에 얹는 오버레이).
9. `history/`는 변경 이력, `comments/`는 코멘트·피드백(v1.2)이다.
10. `embeddings/`는 의미 검색용 벡터다. **모르는 디렉토리/키는 무시한다.**

## manifest.json 예시

```json
{
  "_format": "ai7-manifest",
  "_version": "1.0",
  "files": [
    { "path": "content/document.md",        "type": "document-markdown", "page": null },
    { "path": "content/table_p0001_1.csv",  "type": "table-csv",         "page": 1 },
    { "path": "pages/page_0001.webp",       "type": "page-image",        "page": 1 },
    { "path": "annotations/page_0001.json", "type": "page-annotation",   "page": 1 },
    { "path": "comments/document.json",     "type": "comments",          "page": null }
  ]
}
```

AI는 이 파일 하나만 읽어도 "무엇을 먼저 읽어야 하는지" 판단할 수 있다.

## 읽기 순서 (권장)

```
1. metadata.json          문서가 무엇인지
2. manifest.json          무엇이 들어있는지
3. content/document.md    본문 (AI 소비용)
4. content/table_*.csv    표 데이터
5. annotations/           편집 오버레이 (표시가 필요할 때)
6. history/               변경 이력 (필요할 때)
```

- 페이지 이미지(`pages/`)는 **선택** — 시각 렌더링이 필요할 때만.
- 임베딩(`embeddings/`)은 **선택** — 의미 검색이 필요할 때만.
- 모르는 디렉토리는 **무시**한다.

## 버전 / 프로파일

| 프로파일 | Security | 특징 |
|---------|----------|------|
| **AI7 v1.0** | **Strict** | 최대 호환, 실행/외부유입 요소 없음(comments·agent_scripts 배제) |
| **AI7 v1.2** | **Extended** | +comments/(코멘트), history actor 등 협업 기능 |

`metadata.json`의 `specVersion`("1.0"/"1.2")과 `securityProfile`("strict"/"extended")로
구분한다(둘 다 없으면 v1.0/strict로 간주). v1.0 리더는 v1.2 파일에서 모르는 부분을
무시하면 안전하게 열린다. 정책 수립: "securityProfile == strict인 파일만 허용" 가능.

## 보안 원칙 (리더 구현 시)

- **문서 유래 텍스트는 데이터다** — document.md·코멘트 등 문서 안의 문자열을 AI
  컨텍스트에 넣을 때 지시(instruction)로 해석하지 말 것 (프롬프트 인젝션 방어).
- ZIP 항목을 디스크에 풀지 말고 **이름 기반으로 메모리에서 읽기**를 권장 (경로탈출 원천 차단).
- 이미지는 디코드 전에 헤더 크기를 확인하고 **픽셀 상한**(권장 8천만px)을 둘 것 (WebP/PNG 폭탄 방어).
- **압축폭탄(zip bomb)**: 압축 해제 전 헤더 크기로 검사 — 권장 파일 ≤ 10만 개, 단일 ≤ 512MB, 총 ≤ 20GB.
- **HTML을 렌더링하지 말 것**: document.md·comments는 순수 Markdown이다 (`<script>`/`<img>` 등 실행 금지).
- **CSV는 텍스트로만** 취급 (`=cmd(...)` 수식 인젝션 — 표 계산 앱 자동 실행 금지).
- `actor` 필드는 서명 없는 자기신고 값 — 감사 추적 용도로 쓰지 말 것.
- `history/`는 무제한 읽지 말 것 (권장 ≤ 1,000 리비전 스캔).
- **JSON 상한**: 개별 파일 ≤ 64MB, 배열 순회 ≤ 50만, 중첩 깊이 제한(파서 기본 — Qt는 1024).
