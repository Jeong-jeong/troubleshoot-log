# Troubleshoot Log

> Claude Code와의 트러블슈팅 세션을 **PHARL 형식**으로 문서화하고, 원하는 곳에 저장하세요.

[English](./README.md) | 한국어

---

## PHARL이란?

**PHARL**은 트러블슈팅 세션을 문서화하기 위한 구조화된 형식입니다:

| 단계 | 의미 | 설명 |
|------|------|------|
| **P** | Problem (문제) | 발생한 이슈 - 증상, 재현 단계, 환경 |
| **H** | Hypothesis (가설) | 근거와 검증 방법을 포함한 추정 원인 |
| **A** | Attempt (시도) | 코드 변경과 결과를 포함한 해결 시도 |
| **R** | Result (결과) | Before/After 메트릭 비교가 포함된 결과 |
| **L** | Lesson (교훈) | 기술적 인사이트, 설계 원칙, 액션 아이템 |

이 형식은 **무엇을 했는지뿐만 아니라 왜 했는지**까지 기록하여, 나중에 참고할 때 가치 있는 트러블슈팅 로그를 만들어줍니다.

---

## 주요 기능

- **자동 분석**: Claude Code와의 트러블슈팅 대화를 분석
- **구조화된 문서화**: 포괄적인 PHARL 형식의 마크다운 생성
- **태그 자동 생성**: 내용 기반으로 태그 자동 생성
- **다국어 지원**: 사용자의 언어로 응답 (한국어, 영어 등)
- **유연한 저장 옵션**: Notion, Obsidian, 로컬 파일, 또는 MCP가 지원하는 어디든

---

## 설치

```bash
/plugin install Jeong-jeong/troubleshoot-log
```

---

## 사용법

### 명령어

```
/ts
```

또는 전체 이름:

```
/troubleshoot-log:ts
```

### 트리거 문구

플러그인이 인식하는 패턴:
- `트러블슈팅`, `troubleshooting`
- `PHARL 형식`, `PHARL format`
- `디버깅 세션 문서화`

### 워크플로우

```
┌─────────────────┐
│  Claude와       │
│  트러블슈팅     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  /ts 실행       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Claude가       │
│  대화 분석      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  PHARL 문서     │
│  + 태그 자동    │
│  생성           │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  저장 위치      │
│  선택           │
└────────┬────────┘
         │
    ┌────┼────┐
    ▼    ▼    ▼
┌──────┐┌──────┐┌──────┐
│Notion││로컬  ││ 기타 │
│      ││파일  ││ MCP  │
└──────┘└──────┘└──────┘
```

---

## 출력 예시

```markdown
# API 응답 핸들러의 TypeError

📅 **날짜**: 2025-01-20
🏷️ **태그**: `#NULL_CHECK`, `#API`, `#AXIOS`

---

## 🐛 문제

### 증상
- TypeError: Cannot read property 'data' of undefined
- API 호출 시 간헐적으로 발생

### 환경
- Node.js 18.x, axios 1.6.0

## 🔍 가설

### 가설 1: Race Condition
- **근거**: 에러가 높은 부하에서만 발생
- **추정 원인**: 상태 초기화 전에 응답이 도착

## 🔧 시도

### 시도 1: Null Check 추가

에러 스택 트레이스를 분석한 결과, `response.data`가 undefined일 때 발생하는 것으로 확인되었다. API 응답이 비정상적인 경우를 처리하지 않고 있어서 optional chaining을 적용하기로 했다.

**접근 방식**: undefined 응답에 대한 가드 추가

```javascript
// Before
const user = response.data.user;

// After
const user = response?.data?.user ?? null;
```

**결과**:
```
$ npm test
✓ All 15 tests passed
```
기존에 발생하던 TypeError가 더 이상 발생하지 않음.

## 📊 결과

| 항목 | Before | After |
|------|--------|-------|
| 에러율 | 5% | 0% |

## 💡 교훈

### 기술적 인사이트
- 중첩된 속성에 접근하기 전에 항상 API 응답을 검증할 것
```

---

## 저장 옵션

문서 생성 및 검토 후, 어디에 저장할지 물어봅니다.

### 지원되는 저장소

| 저장소 | 요구사항 |
|--------|----------|
| **Notion** | Notion MCP 설정 필요 |
| **Obsidian** | Obsidian MCP 설정 필요 |
| **로컬 파일** | 없음 (항상 가능) |
| **기타** | 해당 MCP 설정 필요 |

### 로컬 파일 저장

항상 사용 가능한 대안입니다. 기본 위치:
```
./troubleshoot-logs/YYYY-MM-DD-[title-slug].md
```

### Notion 설정 (선택사항)

1. **Notion MCP 추가**
   ```bash
   claude mcp add --transport http notion https://mcp.notion.com/mcp
   ```

2. **인증**
   ```
   /mcp
   ```
   Notion을 선택하고 브라우저에서 인증을 완료하세요.

3. **터미널 재시작**

   최초 설정 후, 변경사항이 적용되도록 터미널을 재시작하세요.

---

## 태그

태그는 트러블슈팅 로그를 분류하고 나중에 찾는 데 도움이 됩니다.

### 작동 방식

1. **자동 생성**: Claude가 트러블슈팅 내용을 분석하여 관련 태그 자동 생성
2. **형식**: `#UPPER_SNAKE_CASE` (예: `#NULL_CHECK`, `#API`, `#REACT`)
3. **수정 가능**: 저장 전 "Other"를 통해 수정 요청 가능

---

## 파일 구조

```
troubleshoot-log/
├── .claude-plugin/
│   ├── plugin.json        # 플러그인 메타데이터
│   └── marketplace.json   # 마켓플레이스 설정
├── commands/
│   └── ts.md              # /ts 명령어 정의
├── README.md              # 영어 문서
└── README.ko.md           # 한국어 문서
```

---

## 문제 해결

### [플랫폼]에 저장 실패
1. 해당 MCP가 설정되어 있는지 확인 (`/mcp`)
2. 필요시 재인증
3. 터미널 재시작
4. 대안으로 로컬에 저장

### 명령어를 찾을 수 없음
```bash
/plugin install Jeong-jeong/troubleshoot-log
```

---

## 라이선스

MIT

---

## 제작자

**Jeong-jeong**

- GitHub: [@Jeong-jeong](https://github.com/Jeong-jeong)
