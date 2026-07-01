# GlobalGates AI Project

이 저장소는 GlobalGates 서비스에 적용할 AI 기능을 노트북 단계에서 실험하고 검증한 기록입니다.

수출입 B2B 플랫폼에서 필요한 게시글 분류, 회원 추천, 무역 규제 문서 질의응답 기능을 데이터 분석과 모델링 관점에서 먼저 확인했고, 검증한 기능은 FastAPI 서버로 옮겨 구현했습니다.

## GlobalGates AI 포트폴리오 흐름

이 저장소는 GlobalGates AI 포트폴리오 중 **프로젝트 적용 실험 단계**에 해당합니다.

1. `study-llm`: LLM, LangChain, RAG를 학습하며 정리한 실습 저장소
2. `globalgates-ai-project`: GlobalGates 서비스에 적용할 AI 기능을 노트북으로 실험한 저장소
3. `globalgates-ai-backend`: 실험한 기능을 FastAPI 기반 API 서버로 구현한 저장소

## 해결하고자 한 문제

GlobalGates는 수출입 기업과 전문가를 연결하는 B2B 서비스입니다. 프로젝트를 진행하면서 다음 기능이 필요하다고 판단했습니다.

- 게시글이 어떤 비즈니스 카테고리에 가까운지 자동 추천
- 회원의 소개글과 관심 카테고리를 바탕으로 연결할 만한 회원 추천
- 국가별 무역 규제 문서를 기반으로 질문에 답하는 RAG 흐름
- 게시글과 채팅 내용을 사용자의 언어로 번역하고, 반복 요청을 캐시

## 실험 흐름

```text
데이터 분석
  -> 카테고리 분류 모델
  -> 팔로우 추천 실험
  -> 무역 규제 RAG 실험
  -> 번역/캐시 실험
  -> FastAPI 서버 구현
```

## 주요 노트북

| 파일 | 내용 |
| --- | --- |
| `export_gap_analysis.ipynb` | 수출입 시장 구조와 중소기업 수출 격차 분석 |
| `globalgates_category_classifier.ipynb` | 게시글/상품/뉴스 텍스트 기반 카테고리 분류 모델 |
| `globalgates_follower_recommender_profile_score.ipynb` | 프로필 완성도 점수를 반영한 회원 추천 실험 |

### 보조 실험 (외부 API·데이터 의존, 출력 미포함)

아래 노트북은 네이버 OpenAPI 키, Redis, 수집한 PDF 등 외부 환경이 있어야 실행되며, 공개 저장소에는 출력 결과를 포함하지 않았습니다.

- `02_collect_naver_openapi.ipynb` — 네이버 OpenAPI 기반 쇼핑 데이터 수집
- `globalgates_translation_chat_post.ipynb` — 게시글/채팅 번역과 Redis 캐시 실험
- `RAG/01_collect_trade_regulation_pdfs.ipynb` — 국가별 무역 규제 PDF 수집
- `RAG/02_build_trade_regulation_rag_redis.ipynb` — PDF 문서 기반 Redis RAG 인덱스 생성
- `03_query_trade_regulation_rag.ipynb` — 생성한 RAG 인덱스 질의 실험

## 결과 요약

- 카테고리 분류 모델은 실험 기준 정확도 약 `0.9406`, F1 약 `0.9408`, AUC 약 `0.9946`을 확인했습니다.
- 추천 실험에서는 회원 bio, 관심 카테고리, 팔로워 수, 프로필 완성도 등을 비교했습니다.
- RAG 실험에서는 국가별 무역 규제 PDF를 수집하고, Redis 기반 검색/질의 흐름을 검증했습니다.
- 번역 실험에서는 DB 조회, 기존 번역 확인, Redis 캐시, LLM 번역 체인을 순서대로 구성했습니다.

## 공개 저장소에서 제외한 항목

다음 항목은 공개 저장소에 포함하지 않았습니다.

- API 키와 DB 접속 정보
- 네이버 OpenAPI 원천 수집 CSV
- 관세청/통계청 등 원천 Excel 파일
- 학습된 모델 파일
- 수집한 PDF 원문 전체
- RAG 저장소와 생성 산출물

필요한 설정 값은 `.env.example`에 예시로만 정리했습니다.

## 한계와 개선 방향

- 노트북 실험은 재현 흐름을 보여주기 위한 것이며, 공개 저장소에는 원천 데이터가 포함되어 있지 않습니다.
- 카테고리 분류 모델은 데이터 수집 방식과 라벨 품질에 영향을 받습니다. 실제 서비스에서는 운영 데이터로 재학습하는 과정이 필요합니다.
- 추천 시스템은 이후 클릭, 팔로우, 메시지 같은 행동 로그를 반영하면 더 자연스러워질 수 있습니다.
- RAG 답변은 출처 문서와 페이지를 구조화해서 반환하도록 개선할 수 있습니다.

## 관련 저장소

- `study-llm`: LLM, LangChain, RAG 학습 정리
- `globalgates-ai-backend`: 이 실험을 FastAPI API 서버로 구현한 저장소

