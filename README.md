# cifar10-generalization-bench

# AlexNet-on-CIFAR10 | Overfitting & Generalization Study

CIFAR-10(32×32 컬러 이미지, 10개 클래스)을 대상으로 **AlexNet-style ConvNet을 직접 구현**하고,  
의도적으로 **overfitting을 유도한 모델**과 **regularization/augmentation을 적용한 모델**을 비교하여  
과적합과 일반화 성능의 차이를 분석합니다.

[Goal]
- AlexNet-style ConvNet을 직접 구현 (no pre-trained / no predefined AlexNet)
- 작은 subset에서 overfitting을 명확히 재현
- 정규화(regularization) + 데이터 증강(augmentation)으로 일반화 성능 개선
- 학습 곡선 및 정성/정량 비교로 변화 분석

---

## 주요 구현 사항

- **Custom AlexNet-style CNN** (CIFAR-10 입력에 맞게 커널/채널/FC 구성 조정)
- **Training pipeline** 구축
  - weight initialization
  - loss / optimizer / mini-batch training
  - epoch별 train/val loss & accuracy 로깅 및 시각화
- **Overfit 실험**
  - train subset(예: 1,000장 이하)만 사용
  - regularization / augmentation 없이 학습
  - train acc가 거의 100%에 수렴하도록 충분히 학습
- **Generalization 개선 실험**
  - CIFAR-10 전체 train set으로 학습
  - regularization 최소 2개 적용
  - augmentation 최소 2개 적용
- **비교 분석**
  - 정량: train/val/test accuracy, 곡선 비교
  - 정성: 정답/오답 이미지 시각화, 학습 동역학/과적합 양상/학습 시간 비교

---

## 실험 구성

### [Baseline / Overfit Model]
- Training data: **small subset (≤ 1,000 images)**
- Regularization: none
- Augmentation: none
- Target: train acc → ~100% (의도적 과적합)

✅ Train Acc (final): 0.9930  
✅ Val Acc (final): 0.4062  
✅ Test Acc (final): 0.4026
✅ Train-Val gap (final): 0.5868

&lt;main point&gt;  
- train 성능이 과도하게 높아지는 반면, val/test 성능이 정체/하락하며 과적합이 발생  
- train/val curve gap(손실/정확도)로 overfitting을 시각적으로 확인  

---

### [Regularized + Augmented Model]
- Training data: **full CIFAR-10 training set**
- Regularization (at least 2):
  - 예: Weight Decay(L2), Dropout, Label Smoothing 등
- Data Augmentation (at least 2):
  - 예: RandomCrop, HorizontalFlip, ColorJitter, Cutout 등

✅ Train Acc (final): 0.9849  
✅ Val Acc (final): 0.8880  
✅ Test Acc (final): 0.8822
✅ Train-Val gap (final): 0.0969

&lt;main point&gt;  
- regularization/augmentation 적용 후 train acc는 낮아질 수 있지만, val/test 성능이 개선되는 경향  
- 곡선의 gap이 줄고, 학습 안정성과 일반화 성능이 좋아지는지 확인  

---

## 비교 분석 (Quantitative & Qualitative)

### Quantitative
- overfit model vs regularized model의 최종 **train/val/test acc 비교**
- 두 모델의 **train/val loss curve**를 같은 plot에서 비교
- 두 모델의 **train/val acc curve**를 같은 plot에서 비교

### Qualitative
- 각 모델별 **correct / incorrect** 예측 이미지 샘플 시각화
- regularization/augmentation이
  - 학습 시간
  - overfitting behavior
