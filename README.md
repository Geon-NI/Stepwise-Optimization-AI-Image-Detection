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
