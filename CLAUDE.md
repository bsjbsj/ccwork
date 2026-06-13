# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 개요

Inflearn 강의 실습용 노트 앱. React 19 + TypeScript + Vite + Tailwind CSS v4 기반. 백엔드는 JSON Server(`db.json`)를 사용하며 REST API로 통신.

- 앱: http://localhost:5173
- API: http://localhost:3001/notes

## 주요 명령어

```bash
npm run dev        # Vite + JSON Server 동시 실행 (concurrently)
npm run server     # JSON Server만 단독 실행
npm run build      # tsc + vite build
npm run lint       # ESLint --fix
npm run format     # Prettier --write
npm test           # vitest run (단발 실행)
npm run test:watch # vitest (watch 모드)
```

## 아키텍처

```
src/
├── api/notes.ts          # fetch 기반 CRUD (JSON Server REST)
├── types/note.ts         # Note 인터페이스 (id, title, content, createdAt, updatedAt)
├── context/NotesContext.tsx  # 전역 상태 + API 호출 통합
└── components/           # UI 레이어 (Layout, NoteList, NoteItem, NoteEditor)
```

**데이터 흐름**: `NotesContext` → API 호출(`src/api/notes.ts`) → JSON Server(`db.json`) → 상태 업데이트 → 컴포넌트 리렌더

**상태 관리 패턴**: `NotesProvider`가 `notes[]`, `loading`, `error`를 관리. CRUD 후 서버 응답값으로 로컬 상태를 직접 갱신(refetch 없음). 컴포넌트는 `useNotes()` 훅으로만 접근.

**선택 상태**: `App.tsx`가 `selectedNoteId`와 `isCreating` 플래그를 소유. `NoteList`와 `NoteEditor`에 props로 전달.

## 테스트 설정

- 테스트 프레임워크: Vitest + jsdom + Testing Library
- 설정 파일: `vite.config.ts` (test 블록에 통합)
- setup 파일: `src/test-setup.ts`

## 강의 맥락

`src/types/note.ts`의 `tags` 필드는 강의 진행 중 추가 예정 (현재 없음). 강의 실습 순서에 따라 기능을 점진적으로 확장하는 구조.
