# AI를 위한 CLI 설계

AI 호출자를 위한 보편적인 CLI 계약과, AI가 그 계약을 일관되게 구현하고 유지하도록 돕는 구조를 제안한다. 언어·프레임워크에 독립적인 가이드다.

좋은 기본값에서 시작한다. 도구 이름만으로 탐색하고, 입력 오류는 명확히 알리며, 일반 결과는 JSON으로 받고, 사용 지식은 CLI에서 읽는다. 상태 관리나 비동기 작업 기능은 필요할 때 추가한다.

각 장은 특정 패키지의 현재 API가 아닌 권장 계약을 설명한다. 모든 `tool` 호출은 설계 예시다. TypeScript 패키지를 사례로 사용할 때는 [참조 구현 지원 범위](08-evidence-and-scope.md#참조-구현의-지원-범위)를 먼저 확인한다.

## 목차

1. [설계 목표](01-design-goals.md)
2. [발견과 사용 지식](02-discovery-and-skills.md)
3. [입력과 실행](03-input-and-execution.md)
4. [결과와 표현](04-results-and-presentation.md)
5. [상태와 후속 행동](05-state-and-actions.md)
6. [제작과 검증](06-authoring-and-testing.md)
7. [조건별 패턴](07-conditional-patterns.md)
8. [근거와 적용 범위](08-evidence-and-scope.md)

## 읽는 순서

호출자가 사용하는 표면은 2–5장, 제작과 검증은 6장에 있다. 7장에서 해당하는 조건별 사례를 선택해 읽는다. 8장은 근거·구현 선택·지원 범위의 한계를 구분한다.

[English](../en/index.md)
