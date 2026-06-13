# mermaid-diagram

**Description**: 프로젝트 구조를 분석하여 Mermaid 다이어그램 HTML을 생성하고 브라우저를 실행하여 시각화합니다.

## Trigger

사용자가 "mermaid", "아키텍처 다이어그램", "구조 시각화", "컴포넌트 의존성 시각화", `/mermaid-diagram` 을 언급할 때 이 스킬을 사용합니다.

## Steps

### 단계 1: 의존성 분석

`src/` 디렉토리를 스캔하여 컴포넌트 간 import 관계를 파악합니다.

- `find src -name "*.ts" -o -name "*.tsx"` 로 파일 목록 수집
- 각 파일의 `import` 구문을 분석하여 의존 관계 추출
- `graph TD` 구문으로 변환: `A[ComponentA] --> B[ComponentB]`
- 레이어별 그룹화: `subgraph` 를 활용해 `components/`, `context/`, `api/`, `types/` 구분

### 단계 2: HTML 파일 생성

`docs/architecture/index.html` 에 Mermaid 다이어그램 HTML을 생성합니다.

- `mkdir -p docs/architecture` 로 디렉토리 보장
- Mermaid.js CDN 포함: `https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js`
- 어두운 테마 적용:
  - 배경색: `#1a1a1a`
  - 텍스트: `#e0e0e0`
  - Mermaid 테마: `dark`
- HTML 구조 예시:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Architecture Diagram</title>
  <style>
    body { background: #1a1a1a; color: #e0e0e0; font-family: sans-serif; padding: 2rem; }
    h1 { color: #a0c4ff; }
    .mermaid { background: #2a2a2a; border-radius: 8px; padding: 1rem; }
  </style>
</head>
<body>
  <h1>Architecture Diagram</h1>
  <div class="mermaid">
graph TD
  %% 분석된 의존성이 여기에 삽입됨
  </div>
  <script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
  <script>mermaid.initialize({ startOnLoad: true, theme: 'dark' });</script>
</body>
</html>
```

### 단계 3: 브라우저 실행

OS를 감지하여 즉시 브라우저를 엽니다.

```bash
# macOS
open docs/architecture/index.html

# Linux
xdg-open docs/architecture/index.html
```

OS 감지: `uname -s` 결과가 `Darwin` 이면 macOS, `Linux` 이면 Linux.

## 완료 보고

모든 단계 완료 후 반드시 다음 메시지를 출력합니다:

> 아키텍처 다이어그램이 브라우저에서 열렸습니다.
