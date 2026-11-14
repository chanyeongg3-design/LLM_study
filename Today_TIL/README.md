# 📓 Today I Learned (TIL)

> 2026년 8월부터 11월까지 매일 공부한 내용을 기록한 학습 일지입니다. 머신러닝에서 시작해 딥러닝, Transformer, LangChain, RAG, LLMOps를 거쳐 KANT 프로젝트까지 이어집니다.

| 기록 수 | 기간 | 부록 |
| :-: | :-: | :-: |
| **44편** | 2026-08-11 ~ 2026-11-14 | 통합 가이드 1편 |

## 🧭 학습 흐름

```text
머신러닝 → 딥러닝/PyTorch → Transformer → LLM API → LangChain → RAG → LLMOps → KANT Project 3
```

| 단계 | 주제 | 기간 | 편수 |
| :-: | :--- | :--- | :-: |
| 1 | [머신러닝](#phase-1) | 08/11 ~ 08/18 | 5 |
| 2 | [딥러닝 · PyTorch](#phase-2) | 08/19 ~ 09/01 | 9 |
| 3 | [Transformer · LLM 기초](#phase-3) | 09/02 ~ 09/11 | 7 |
| 4 | [LLM 비교 실험 (KANT Project 1)](#phase-4) | 09/15 ~ 09/16 | 2 |
| 5 | [RAG 파이프라인 · API 안정화](#phase-5) | 09/23 ~ 09/24 | 2 |
| 6 | [LangChain](#phase-6) | 09/29 ~ 10/02 | 4 |
| 7 | [분류 모델 · 은행 상담 프로젝트](#phase-7) | 10/07 ~ 10/13 | 3 |
| 8 | [프롬프트 엔지니어링 (KANT Project 2)](#phase-8) | 10/23 ~ 10/23 | 1 |
| 9 | [RAG 심화 · Context Engineering](#phase-9) | 10/26 ~ 10/29 | 5 |
| 10 | [LLMOps (Langfuse · 평가)](#phase-10) | 10/30 ~ 11/03 | 3 |
| 11 | [KANT Project 3 (Retriever)](#phase-11) | 11/05 ~ 11/14 | 3 |

---

<a id="phase-1"></a>

## 1. 머신러닝

| 날짜 | 주제 | 핵심 내용 |
| :--- | :--- | :--- |
| [2026-08-11](<./2026-08-11.md>) | 머신러닝 1강 1장 | 문제 유형(회귀·분류·군집·차원축소), train/valid/test 분리, 회귀 평가지표 |
| [2026-08-12](<./2026-08-12.md>) | 머신러닝 2장 3강 | 배깅과 랜덤포레스트 (의사결정나무의 높은 분산 문제) |
| [2026-08-13](<./2026-08-13.md>) | 머신러닝 3장 | 편향·분산, 학습곡선·검증곡선, 규제(Ridge/Lasso/ElasticNet) |
| [2026-08-14](<./2026-08-14.md>) | 머신러닝 4장 | 머신러닝 4장 학습 정리 |
| [2026-08-18](<./2026-08-18.md>) | 머신러닝 5장 | Grid Search vs Random Search, Pipeline, CV와 Test의 차이 |

<a id="phase-2"></a>

## 2. 딥러닝 · PyTorch

| 날짜 | 주제 | 핵심 내용 |
| :--- | :--- | :--- |
| [2026-08-19](<./2026-08-19.md>) | 딥러닝 1장~2장 2강 | Tensor(shape·dtype), Batch, unsqueeze/squeeze, Broadcasting |
| [2026-08-20](<./2026-08-20.md>) | 딥러닝 2-3강~3장 | Device, 퍼셉트론 |
| [2026-08-21](<./2026-08-21.md>) | 딥러닝 4장 | 활성화 함수와 ReLU |
| [2026-08-24](<./2026-08-24.md>) | 딥러닝 5장: 손실 함수와 최적화 | MSE·MSELoss, SGD·Adam, PyTorch 학습 step |
| [2026-08-25](<./2026-08-25.md>) | PyTorch Autograd와 Dataset·DataLoader | requires_grad, 자동 미분, Dataset/DataLoader |
| [2026-08-26](<./2026-08-26.md>) | Transform과 이미지 데이터 전처리 | Train/Validation 전처리 분리, 이미지 Transform |
| [2026-08-28](<./2026-08-28.md>) | 딥러닝 10-3강~11-6강 복습 | 과적합 진단, Dropout, BatchNorm, 학습·평가 모드 차이 |
| [2026-08-31](<./2026-08-31.md>) | CNN 실험부터 RNN/LSTM까지 | CNN의 공간 구조 활용, RNN·LSTM, 계산 그래프 주의점 |
| [2026-09-01](<./2026-09-01.md>) | 딥러닝 7장 전체 실습 | TensorDataset·DataLoader 실습 문제 |

<a id="phase-3"></a>

## 3. Transformer · LLM 기초

| 날짜 | 주제 | 핵심 내용 |
| :--- | :--- | :--- |
| [2026-09-02](<./2026-09-02.md>) | Transformer가 필요한 이유와 NLP 데이터 파이프라인 | RNN/LSTM의 한계, 미니 LLM 워크플로우, softmax |
| [2026-09-03](<./2026-09-03.md>) | Self-Attention과 Multi-Head Attention Shape | Query·Key·Value, [B, L, D_MODEL] shape |
| [2026-09-04](<./2026-09-04.md>) | Transformer 전체 구조 | Encoder/Decoder 구조, Causal·Padding Mask, FFN, Residual, LayerNorm, Prefill/Decode |
| [2026-09-08](<./2026-09-08.md>) | 분류 모델 Fine-tuning과 Trainer | Hugging Face datasets, Trainer 기반 파인튜닝 |
| [2026-09-09](<./2026-09-09.md>) | 모델 개선·평가와 답변 생성 제어 | 튜닝 기법 구분, 학습 vs 생성, 형식 준수율 vs 정답률, 디코딩 파라미터, Chat Template |
| [2026-09-10](<./2026-09-10.md>) | Loss Function과 Optimizer | 손실 함수, softmax overflow 방지, Optimizer |
| [2026-09-11](<./2026-09-11.md>) | Responses API, Structured Output, Tool Calling | LLM API 호출, 구조화 출력, 도구 호출 |

<a id="phase-4"></a>

## 4. LLM 비교 실험 (KANT Project 1)

| 날짜 | 주제 | 핵심 내용 |
| :--- | :--- | :--- |
| [2026-09-15](<./2026-09-15.md>) | 코드 생성 LLM 비교 실험 | qwen2.5-coder vs deepseek-coder: 버그 수정 10문제 비교 |
| [2026-09-16](<./2026-09-16.md>) | KANT Project 1 모델 비교 | Qwen(Local) vs Cloud 모델, 실패 원인 분석 |

<a id="phase-5"></a>

## 5. RAG 파이프라인 · API 안정화

| 날짜 | 주제 | 핵심 내용 |
| :--- | :--- | :--- |
| [2026-09-23](<./2026-09-23.md>) | RAG 데이터 파이프라인과 FastAPI 기초 | 데이터 정제, RAG 문서 구조, FastAPI·Uvicorn, REST, Pydantic (본문 날짜 09-22) |
| [2026-09-24](<./2026-09-24.md>) | 외부 LLM API 호출 안정화 | Timeout, Retry, 502 등 오류 처리, Middleware 요청 로그 (본문 날짜 09-23) |

<a id="phase-6"></a>

## 6. LangChain

| 날짜 | 주제 | 핵심 내용 |
| :--- | :--- | :--- |
| [2026-09-29](<./2026-09-29.md>) | LangChain | Prompt Template, Model, Output Parser, python-dotenv·API Key 관리 |
| [2026-09-30](<./2026-09-30.md>) | LangChain 기본 체인부터 Retrieval Chain까지 | Structured Output, Pydantic, Retrieval Chain |
| [2026-10-01](<./2026-10-01.md>) | LangChain 통합: 대화 기억·스트리밍·복구 | session_id, MessagesPlaceholder, Streaming/Callback, Retry/Fallback, 문서 Q&A |
| [2026-10-02](<./2026-10-02.md>) | Python으로 LLM API 호출·프롬프트 평가 | RUN_LIVE, Basic/Rule/Few-shot 프롬프트, JSON 파싱, accuracy·format_rate |

<a id="phase-7"></a>

## 7. 분류 모델 · 은행 상담 프로젝트

| 날짜 | 주제 | 핵심 내용 |
| :--- | :--- | :--- |
| [2026-10-07](<./2026-10-07.md>) | 저장한 분류 모델을 LLM 서비스 흐름으로 연결 | 모델 선택·Checkpoint 복원, 분류 모델 추론 흐름, Tokenizer |
| [2026-10-08](<./2026-10-08.md>) | 은행 상담 데이터와 분류·생성 프로젝트 설계 | 데이터 구성, 긴 상담 처리, 베이스라인과 모델 비교 |
| [2026-10-13](<./2026-10-13.md>) | 개발 일지: BERT 파인튜닝 점검과 비교 질문 준비 | FileNotFoundError 해결, 5 epoch 재학습(test Macro F1 0.7047), 비교용 질문 24개 |

<a id="phase-8"></a>

## 8. 프롬프트 엔지니어링 (KANT Project 2)

| 날짜 | 주제 | 핵심 내용 |
| :--- | :--- | :--- |
| [2026-10-23](<./2026-10-23.md>) | 프롬프트 엔지니어링 프로젝트 회고 (DevDesk) | JSON Schema, Tool Calling, 검증·재시도, 신뢰할 수 있는 답변 |

<a id="phase-9"></a>

## 9. RAG 심화 · Context Engineering

| 날짜 | 주제 | 핵심 내용 |
| :--- | :--- | :--- |
| [2026-10-26](<./2026-10-26.md>) | RAG: 근거 기반 답변과 검색 성능 개선 | BM25, Dense, Hybrid, RRF, Re-ranking, Query Rewrite, Multi-Query |
| [2026-10-27](<./2026-10-27.md>) | RAG: 평가 기준과 실패 원인별 개선 | Golden Dataset, Hit@k·Recall@k, RAGAS, Corrective/Agentic RAG, GraphRAG |
| [2026-10-29](<./2026-10-29.md>) | RAG 전체 학습 노트 + Context Engineering (통합본) | RAG 아키텍처, 청킹, 임베딩, Qdrant, Context Engineering |
| [2026-10-29 (1)](<./2026-10-29-1.md>) | RAG 전체 학습 노트 | RAG 이해, 문서 로딩, 청킹, 임베딩·Dense 검색, Qdrant |
| [2026-10-29 (2)](<./2026-10-29-2.md>) | Context Engineering | Context 구성, Memory/Mem0, Token Budget, Sliding Window |

<a id="phase-10"></a>

## 10. LLMOps (Langfuse · 평가)

| 날짜 | 주제 | 핵심 내용 |
| :--- | :--- | :--- |
| [2026-10-30](<./2026-10-30.md>) | LLMOps 1장: Langfuse 관측과 오류 분석 | Trace·Observation·Span, 실패 원인 분류 |
| [2026-11-02](<./2026-11-02.md>) | LLMOps 2장: Golden Dataset과 회귀 검증 | 프롬프트 버전 관리, Langfuse 평가, Baseline vs Candidate, Holdout |
| [2026-11-03](<./2026-11-03.md>) | LLMOps 3장: 운영 지표와 사용자 피드백 | 운영 지표 계산, 사용자 피드백, 개선 판단 |

<a id="phase-11"></a>

## 11. KANT Project 3 (Retriever)

| 날짜 | 주제 | 핵심 내용 |
| :--- | :--- | :--- |
| [2026-11-05](<./2026-11-05.md>) | KANT PROJECT 3 – DAY 1 개발 일지 | 프로젝트 구조, Retriever 세 가지 검색 방식, 개발 전 준비 |
| [2026-11-12](<./2026-11-12.md>) | Retriever 전체 정리 | Naive/Hybrid/Hybrid+Rerank, 실험 결과, Latency, 면접 질문 10가지 |
| [2026-11-14](<./2026-11-14.md>) | KANT Project 3 마무리: Retriever 비교와 회고 | Naive/Hybrid/Hybrid+Rerank 결과, k=10 선택, 팀워크·협업에서 배운 점 |

---

## 📚 부록

| 문서 | 설명 |
| :--- | :--- |
| [KANT PROJECT 3 통합 가이드](<./KANT_PROJECT3_통합가이드.md>) | PDF 기반 RAG Q&A 서비스 구축과 검색 방식 비교를 정리한 복습 교재 (Retriever 담당자용) |

## ℹ️ 참고

- `2026-09-23.md`, `2026-09-24.md`는 파일 이름과 본문 제목의 날짜(09-22, 09-23)가 하루씩 다릅니다.
- `2026-10-29.md`는 `2026-10-29-1.md`(RAG)와 `2026-10-29-2.md`(Context Engineering)를 합친 통합본에 해당합니다.
- 이미지는 [`images/`](<./images/>) 폴더에 있습니다.
