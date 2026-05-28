# Stepwise-Optimization-AI-Image-Detection
CIFAKE(저해상도)에서 GenImage(고해상도) 데이터셋으로의 단계적 학습을 통한 범용적 AI 생성 이미지 탐지 및 분류 모델 최적화 프로젝트


# 🤖 Progressive-AI-Image-Classifier

> **단계적 학습 기반의 AI 생성 이미지 분류 모델 최적화 및 일반화**
> 
> **팀원:** 고영훈 (C289002), 김건우 (C289003)

---

## 📌 1. 문제 정의 (Problem Definition)

### 1.1 배경 및 필요성
* **생성형 AI의 급성장:** 최근 Stable Diffusion, Midjourney 등 이미지 생성 AI 기술의 발전으로 정교한 합성 이미지(AI-generated)가 급증하고 있습니다.
* **식별의 어려움:** 생성형 이미지는 육안으로 실제 이미지와 구분하기 어려울 정도로 정교해져, 딥페이크나 가짜 뉴스 등 다양한 사회적 문제를 야기합니다.

### 1.2 기존 접근법의 한계
* **막대한 연산 비용:** 초기부터 고해상도 데이터를 직접 학습할 경우, 많은 GPU 자원과 긴 학습 시간이 소요됩니다.
* **과적합(Overfitting)과 일반화 실패:** 모델이 고해상도 이미지의 미세한 픽셀 패턴(Texture)에만 과하게 집중하면, 화질이 낮거나 다른 방식으로 생성된 알고리즘의 이미지를 만났을 때 분류 성능이 급격히 떨어집니다.

### 1.3 프로젝트 해결 방안
본 프로젝트는 모델이 데이터의 복잡도를 단계별로 극복하게 하여 **학습 효율성**과 **일반화 성능**을 동시에 확보합니다.

1. **[Phase 1] CIFAKE를 활용한 기초 특징 학습:** 저해상도 데이터를 사용하여 이미지의 전반적인 구도와 형태(Global Structure)를 우선 학습하고 기초 가중치를 빠르게 수렴시킵니다.
2. **[Phase 2] 점진적 해상도 확장:** 해상도를 단계별로 높여가며 Convolution 연산이 포착하는 세부 질감(Fine Details)을 학습합니다.
3. **[Phase 3] 도메인 확장을 통한 일반화:** 특정 알고리즘에 편향되지 않도록 다양한 생성 모델 결과물이 포함된 데이터를 투입하여, 보지 못한(Unseen) 데이터에 대해서도 정확히 분류하는 '범용적 탐지 능력'을 검증합니다.

---

## 📊 2. 데이터셋 (Datasets)

* **메인 데이터셋: CIFAKE (Real and AI-Generated Synthetic Images)**
  * **출처:** Kaggle (`birdy654/cifake-real-and-ai-generated-synthetic-images`)
  * **구성:** 총 120,000장 (Real 6만 / Fake 6만)
  * **특징:** CIFAR-10 기반의 저해상도 ($32 \times 32$) 이미지로, 초기 단계 학습 및 모델 일반화 기초 구축에 적합합니다.
* **확장 데이터셋: Unbiased Tiny GenImage / AI vs Real Images Dataset**
  * Midjourney, Stable Diffusion, GAN 등 다양한 알고리즘으로 생성된 고해상도 데이터를 활용하여 모델의 도메인 확장 및 범용 성능을 검증합니다.

---

## 🛠 3. 개발 환경 및 프로젝트 아키텍처

* **Language:** Python
* **Framework:** TensorFlow / Keras (Sequential API)
* **Environment:** Google Colab
* **핵심 구조:** CNN (Convolutional Neural Network) 기반 이미지 분류 모델

---

## 🤝 4. 팀원 역할 분담

병렬적 연구 구현 및 상호 보완적 협업 체계로 진행되었습니다.

* **김건우 (데이터 및 파이프라인 마스터)**
  * Kaggle API 환경 세팅 및 데이터셋 다운로드 자동화 스크립트 작성
  * NumPy/Pandas를 활용한 $[0, 1]$ 정규화 및 데이터 분할(8:1:1) 파이프라인 구축
  * 단계별 고해상도 학습을 위한 이미지 해상도 변경 조절 함수 구현
* **고영훈 (모델 설계 및 최적화 엔지니어)**
  * TensorFlow Keras Sequential API 기반 기초 CNN Baseline 모델 설계
  * 학습 규칙(Loss, Optimizer) 설정 및 하이퍼파라미터 튜닝
  * 점진적 해상도 확장 시 학습 균형을 위한 학습률 스케줄러(Learning Rate Scheduler) 구현
  * 과적합 방지를 위한 Dropout 및 데이터 증강(Data Augmentation) 적용

---

## 📅 5. 프로젝트 수행 일정 (10일 초압축 마일스톤)

* **1~3일차: 데이터 고속도로 개통 및 기초 학습 (Phase 1)**
  * Kaggle 연동 파이프라인 구축 및 저해상도 ($32 \times 32$) Baseline 모델 설계 및 첫 수렴 확인
* **4~6일차: 해상도 업그레이드 및 심화 학습 (Phase 2)**
  * 데이터를 단계별로 리사이징하는 전처리 모듈 적용 및 해상도 확장에 따른 모델 미세 조정(Fine-tuning)
* **7~8일차: 실전 테스트 및 도메인 확장 검증 (Phase 3)**
  * 미학습(Unseen) 확장 데이터셋을 투입하여 범용 탐지 능력 검증 및 오분류 분석
* **9~10일차: 최종 성능 비교 및 보고서 작성**
  * 모델별 성능 지표(학습 곡선, 정확도, Loss) 시각화 및 최종 결론 도출
