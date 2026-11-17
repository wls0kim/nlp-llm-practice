# 텍스트 분석과 LLM 활용 실습

토픽 모델링, LLM 합성 데이터, GPT-4o 멀티모달 분류를 다룬 분석 코드

- 대학원 과정에서 직접 작성한 분석 코드
- 원자료, 결과 파일, 보고서 원문은 비공개

| 폴더 | 내용 | 방법 | 도구 |
|---|---|---|---|
| [stm-public-servant-reviews](stm-public-servant-reviews) | 공무원 온라인 리뷰 토픽 분석 | 구조적 토픽 모델링(STM), 공변량 효과 | R |
| [llm-synthetic-data](llm-synthetic-data) | LLM으로 만든 합성 데이터로 분류 모형 학습 | GPT few-shot, zero-shot 데이터 생성, TF-IDF 분류 | Python |
| [meme-sentiment-gpt4o](meme-sentiment-gpt4o) | 밈 이미지 감성 분류 | GPT-4o 멀티모달 프롬프트, 성능 평가 | Python |

## 방법 설명

- **구조적 토픽 모델링(STM)**: 문서에 섞여 있는 주제를 자동으로 찾고, 집단(예: 만족도 고저)에 따라 주제 비중이 어떻게 다른지까지 추정하는 텍스트 분석 방법
- **TF-IDF**: 한 문서에 자주 나오면서 다른 문서에는 드문 단어에 높은 값을 주어 텍스트를 숫자로 바꾸는 방법
- **few-shot, zero-shot**: LLM에 예시를 몇 개 보여 주고(few-shot) 또는 예시 없이(zero-shot) 결과를 생성하게 하는 방식
- **멀티모달 프롬프트**: 텍스트와 이미지를 함께 넣어 LLM에 판단을 요청하는 방식

## 공무원 온라인 리뷰 STM

`stm_report.Rmd`, `figures/`

- 공무원 온라인 기업 리뷰의 장점, 단점 텍스트를 형태소 분석(사용자 사전, 동의어, 불용어 정의)으로 정제
- 별점을 저만족과 고만족으로 나눠 공변량으로 넣고 구조적 토픽 모델링(STM) 적합, `searchK`로 토픽 수 결정
- 만족 수준에 따라 토픽 비중이 유의하게 달라지는지 추정해 시각화

| 그림 | 내용 |
|---|---|
| `figures/pros_topic_summary.png`, `figures/cons_topic_summary.png` | 장점, 단점 리뷰의 토픽별 비중과 대표 단어 |
| `figures/pros_reg_sig_highlight.png`, `figures/cons_reg_sig_highlight.png` | 만족 수준에 따른 토픽 비중 차이 (유의한 토픽 강조) |

## LLM 합성 데이터로 분류 모형 학습

`01_synthetic_data_classifier.ipynb`

- GPT로 한국어 영화 리뷰 감성 데이터를 few-shot 방식(실제 리뷰 예시 제시)과 zero-shot 방식(예시 없이 생성)으로 각각 만듦
- 실제 데이터(NSMC), few-shot 합성 데이터, zero-shot 합성 데이터로 같은 TF-IDF 로지스틱 회귀 분류기를 학습해 정확도 비교
- 실행 시 환경변수 `OPENAI_API_KEY`에 API 키 필요

## GPT-4o로 밈 감성 분류

`meme_sentiment_gpt4o.ipynb`

- 이미지와 그 안의 글이 함께 감성을 만드는 밈 자료를 GPT-4o로 긍정, 부정 분류
- 같은 밈에 텍스트만, 이미지만, 둘 다, 둘 다와 한국 예능 맥락 설명의 네 가지 프롬프트를 무작위 순서로 실행하고, 입력 방식별 성능과 실행 순서 효과를 비교
- 이미지 인코딩, 프롬프트 구성, API 호출과 오류 처리를 클래스로 묶어 전체 이미지를 일괄 분석
- 정답 라벨과 비교해 정확도, 정밀도, 재현율, F1, 혼동행렬을 계산
- 실행 시 환경변수 `OPENAI_API_KEY`에 API 키 필요
