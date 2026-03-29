# Notion Log

`/coach wrap` 실행 시 Notion에 오늘 세션 내용을 저장하는 규칙이다.

## 저장 위치

**데이터베이스**: Challenge1 일지
- URL: https://www.notion.so/0sunset0/33201ce28fac8060a494c439cb6bc460
- data_source_id: `33201ce2-8fac-807d-abbf-000b45b3a442`

## 데이터베이스 속성 스키마

| 속성명 | 타입 | 설명 |
|--------|------|------|
| 이름 | title | 세션 제목 (날짜 없이 주제만, e.g. `SwiftUI @State 이해`) |
| 날짜 | date | 세션 날짜 (ISO-8601, e.g. `2026-03-29`) |
| 주제 | multi_select | `SwiftUI`, `Swift`, `Architecture`, `Debugging`, `Git`, `기타` 중 선택 |

## 저장 형식 (페이지 content)

```
## 오늘 배운 내용
- [대화에서 다룬 개념 1]
- [대화에서 다룬 개념 2]
- ...

## 퀴즈 기록

| 문제 | 정답 |
|------|------|
| [문제] | [정답] |
```

퀴즈 기록은 문제와 정답만 기록한다. 사용자의 답변이나 정답 여부는 포함하지 않는다.

## 저장 절차

1. 오늘 대화에서 다룬 개념들을 정리한다 (3-7개)
2. 퀴즈가 있었으면 문제와 정답만 정리
3. 다룬 주제에 맞는 multi_select 값 결정
4. `mcp__claude_ai_Notion__notion-create-pages`로 data_source_id에 새 페이지 생성
   - parent: `{"type": "data_source_id", "data_source_id": "33201ce2-8fac-807d-abbf-000b45b3a442"}`
   - properties: `이름`, `date:날짜:start`, `주제`
5. 저장 완료 후 사용자에게 Notion 링크 제공

## 새 데이터베이스 속성 추가가 필요한 경우

주제 태그가 부족하면 `mcp__claude_ai_Notion__notion-update-data-source`로 추가:
```
ALTER COLUMN "주제" SET MULTI_SELECT('기존옵션', ..., '새옵션':color)
```

## Notion MCP가 없는 경우

터미널에 저장 내용을 출력하고, 사용자가 직접 복사할 수 있도록 안내한다.
