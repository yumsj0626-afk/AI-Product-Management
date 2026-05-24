
# Managing Machine Learning Projects

## Module 5 — Model Lifecycle Management

### ML System Monitoring / Model Maintenance / Model Versioning / Organizational Considerations

---

## 한 줄 요약

ML 모델은 배포했다고 끝나는 것이 아니라, 시간이 지나며 데이터 분포와 환경이 변하면서 성능이 떨어질 수 있습니다. 따라서 운영 중인 모델은 **입력 데이터, 데이터 파이프라인, 모델 출력, 실제 target label, subgroup 성능, feature 영향도**를 지속적으로 모니터링하고, 성능이 임계값 아래로 떨어지면 retraining 또는 updating을 수행하며, model versioning을 통해 rollback, shadow release, champion-challenger testing이 가능하도록 관리해야 합니다.

---

# 1. 왜 배포된 ML 모델은 계속 관리해야 하는가

일반 소프트웨어는 배포 후 버그를 고치고 기능을 개선하는 정도로 관리할 수 있습니다. 하지만 ML 모델은 배포 이후에도 주변 환경이 계속 변하면서 성능이 자연스럽게 떨어질 수 있습니다.

문제는 ML 모델의 실패가 항상 즉시 눈에 보이지 않는다는 점입니다. 사용자는 잘못된 예측을 받고 있어도 그것이 모델 문제인지 모를 수 있고, 운영팀도 성능 저하를 늦게 발견할 수 있습니다. 그래서 production model에는 별도의 monitoring system이 필요합니다.

```text
모델 배포
→ 현실 환경 변화
→ input data 변화
→ target label 변화
→ input-output 관계 변화
→ model performance degradation
→ monitoring / retraining / updating 필요
```

---

# 2. ML System Monitoring

## 핵심 개념

ML 시스템 모니터링은 모델 output만 보는 것이 아닙니다. 모델에 들어가기 전의 input data, data pipeline을 지난 후의 processed data, 모델의 prediction, 실제 target label, subgroup별 성능, feature 영향도까지 함께 봐야 합니다.

```text
Input Data
→ Data Pipeline
→ Processed Features
→ Model Prediction
→ Actual Target / Label
→ Performance & Drift Monitoring
```

---

## 무엇을 모니터링해야 하는가

| 모니터링 대상 | 확인할 것 | 목적 |
| :--- | :--- | :--- |
| Input Data | schema, encoding, volume, missing data, distribution | 데이터 품질과 data drift 감지 |
| Data Pipeline Output | 전처리 후 feature 값과 분포 | pipeline 처리 오류 감지 |
| Model Output | prediction 분포, performance metric 변화 | 성능 저하 감지 |
| Target Labels | 실제 label 분포, prediction과의 차이 | concept drift 감지 |
| Subgroup Performance | demographic group 등 그룹별 성능 | bias와 불균형 성능 감지 |
| Feature Impact | 어떤 feature가 prediction을 주도하는지 | 비정상적인 모델 의존성 감지 |

---

## Input Data Monitoring

입력 데이터 모니터링에서는 가장 먼저 기본적인 품질 검사를 해야 합니다. schema가 바뀌지 않았는지, encoding이 맞는지, 예상한 양의 데이터가 들어오고 있는지, missing data가 갑자기 늘지 않았는지 확인해야 합니다.

또한 input feature의 분포가 과거 training data 또는 이전 production data와 달라지고 있는지도 봐야 합니다. 예를 들어 각 feature의 mean, variance를 이전 수준과 비교하거나, 시각화를 통해 분포 변화를 확인할 수 있습니다. 이런 변화는 **data drift**의 신호일 수 있습니다.

```text
Input Data Checks
→ schema / encoding
→ data volume
→ missing data
→ feature distribution
→ mean / variance 변화
→ data drift 가능성 확인
```

자동화된 테스트만 믿으면 안 되고, 주기적으로 사람이 직접 input data를 audit하는 것도 필요합니다. 자동 검사에서 놓치는 이상한 패턴이 있을 수 있기 때문입니다.

---

## Data Pipeline Monitoring

운영 환경에서는 live input data가 data pipeline을 거쳐 모델에 들어갑니다. 이때 training data를 처리했던 방식과 live data를 처리하는 방식 사이에 차이가 생기면 문제가 발생할 수 있습니다.

따라서 pipeline을 지난 후의 feature 값도 따로 모니터링해야 합니다. continuous feature는 값이 예상 범위 안에 있는지, 분포가 과거와 비슷한지 확인해야 하고, categorical feature는 training data에 없던 새로운 category가 갑자기 들어오지 않았는지 확인해야 합니다.

```text
Raw Input
→ Data Pipeline
→ Processed Feature

확인할 것:
- continuous feature의 min/max range
- processed feature distribution
- categorical feature의 unknown category
- training data와 live data processing 차이
```

---

## Model Output Monitoring

모델 자체의 성능은 시간이 지남에 따라 떨어질 수 있습니다. 따라서 production model의 performance metric을 시간에 따라 추적하고, 어느 수준 아래로 떨어지면 조치를 취할지 threshold를 미리 정해야 합니다.

```text
초기 성능
→ 시간이 지나며 decay
→ acceptable threshold 접근
→ retraining / updating 필요
```

모델 output의 분포도 중요합니다. prediction label의 분포와 실제 observed label의 분포가 크게 달라지면 concept drift나 bias가 발생했을 가능성이 있습니다.

---

## Target Label과 Concept Drift

target label을 사용할 수 있다면, prediction과 실제 label을 비교해야 합니다. 특히 input feature와 target 사이의 관계가 바뀌는지를 확인하는 것이 중요합니다. 이것이 **concept drift**입니다.

예를 들어 과거에는 어떤 feature가 target과 강하게 양의 상관을 가졌는데, 시간이 지나며 그 상관이 약해지거나 반대로 바뀐다면 모델이 학습한 관계가 더 이상 현실과 맞지 않을 수 있습니다.

```text
과거:
Feature A ↑ → Target ↑

현재:
Feature A ↑ → Target 변화 없음 또는 Target ↓

→ concept drift 가능성
```

---

## Subgroup Performance Monitoring

모델 전체 성능이 좋아 보여도 특정 subgroup에서는 성능이 나쁠 수 있습니다. 예를 들어 전체 accuracy는 높지만, 특정 인구통계 집단에서만 오류율이 높다면 bias 문제가 생길 수 있습니다.

따라서 aggregate performance만 보는 것이 아니라, population을 여러 subgroup으로 나누어 성능을 확인해야 합니다.

```text
전체 성능이 좋음
≠ 모든 사용자 그룹에서 성능이 좋음
```

이 관점은 모델의 공정성, 신뢰성, 실제 사용자 경험을 관리하는 데 중요합니다.

---

## Feature Impact Monitoring

모델이 어떤 feature를 근거로 예측하는지도 주기적으로 확인해야 합니다. 특정 feature가 비정상적으로 큰 영향을 주고 있거나, 논리적으로 중요하지 않아 보이는 feature가 예측을 지배한다면 bias나 잘못된 학습의 신호일 수 있습니다.

단순한 linear model이나 decision tree는 feature 영향도를 비교적 쉽게 볼 수 있습니다. 복잡한 neural network에서는 직접 해석이 어렵지만, SHAP이나 LIME 같은 도구를 사용해 feature contribution을 분석할 수 있습니다.

```text
Feature Impact Monitoring
→ 모델이 어떤 feature에 의존하는가?
→ 그 의존성이 논리적으로 타당한가?
→ 특정 feature가 과도하게 prediction을 지배하는가?
→ bias나 비정상 패턴이 있는가?
```

---

# 3. Model Maintenance

## 핵심 개념

Model maintenance는 monitoring 결과를 바탕으로 모델 성능이 acceptable threshold 아래로 떨어지지 않도록 관리하는 과정입니다. 모든 모델은 시간이 지나면 성능이 떨어질 수 있으므로, 이를 전제로 maintenance cycle을 설계해야 합니다.

```text
Monitor Production Model
→ Performance Decay 확인
→ Threshold 아래로 하락
→ Retrain or Update
→ Evaluate New Version
→ Deploy
→ 다시 Monitoring
```

---

## Retraining vs Updating

모델 성능이 떨어졌을 때 취할 수 있는 조치는 크게 retraining과 updating으로 나뉩니다.

| 구분 | 의미 | 바뀌는 것 | 적합한 상황 |
| :--- | :--- | :--- | :--- |
| Retraining | 기존 모델 구조를 유지하고 새 데이터로 다시 학습 | model weights | 환경 변화에 적응해야 할 때 |
| Updating | 모델링 과정을 다시 수행해 구조 자체를 바꿈 | algorithm, hyperparameter, feature set, weights | 기존 모델 형태가 더 이상 적합하지 않을 때 |

Retraining은 같은 알고리즘과 hyperparameter를 유지한 채, 새로 수집된 데이터로 모델의 weight를 다시 학습하는 방식입니다. 모델이 최근 데이터와 환경 변화를 반영하도록 만드는 데 유용합니다.

Updating은 더 큰 변화입니다. 새 데이터를 바탕으로 feature set을 다시 검토하고, algorithm이나 hyperparameter도 바꿀 수 있습니다. 더 나은 성능을 내는 모델 구조를 찾거나, 더 이상 유효하지 않은 feature를 제거할 수 있습니다.

---

## 왜 Retraining이 필요한가

Retraining은 단순히 성능을 조금 더 올리기 위한 작업이 아닙니다. production 환경에서는 시간이 지나며 현실이 바뀌기 때문에 모델도 이를 반영해야 합니다.

```text
Retraining이 필요한 이유:
- production 중 새 데이터가 계속 쌓임
- 외부 환경이 변함
- data drift와 concept drift가 발생함
- adversarial actor가 모델을 회피하려고 행동을 바꿈
- 최근 데이터가 오래된 데이터보다 더 중요할 수 있음
```

예를 들어 spam detection 모델에서는 spammer들이 기존 모델을 피하기 위해 새로운 단어와 패턴을 사용할 수 있습니다. 이 경우 모델을 재학습하지 않으면 새로운 형태의 spam을 잡지 못합니다.

---

## Scheduled Retraining

Scheduled retraining은 정해진 주기마다 모델을 재학습하는 방식입니다. 예를 들어 매주, 매월, 분기별, 연간 단위로 재학습할 수 있습니다.

이 방식을 제대로 쓰려면 모델의 decay rate를 이해해야 합니다. 모델 성능이 얼마나 빠르게 떨어지는지 알아야, 다음 재학습 전에 성능이 acceptable threshold 아래로 떨어지지 않도록 주기를 정할 수 있습니다.

Scheduled retraining은 수동 작업이 포함된 경우에 특히 자주 사용됩니다. 예를 들어 재학습용 데이터를 사람이 수집하거나 정리해야 한다면, 정해진 일정에 맞춰 준비하는 방식이 현실적입니다.

---

## Triggered Retraining

Triggered retraining은 모델 성능이 미리 정한 threshold 아래로 떨어졌을 때 자동으로 재학습을 시작하는 방식입니다.

```text
Performance Monitoring
→ 성능이 threshold 아래로 하락
→ retraining process 자동 시작
→ 새 모델 평가
→ 배포 여부 결정
```

이 방식은 모델이 변화하는 환경에 더 빠르게 반응할 수 있다는 장점이 있습니다. 다만 triggered retraining이 잘 작동하려면 데이터 수집, 정제, feature 생성, 평가, 배포 과정이 상당 부분 자동화되어 있어야 합니다.

---

## Continuous Learning / Online Learning

Continuous learning은 triggered retraining의 한 형태로 볼 수 있습니다. 새 데이터 포인트 또는 작은 batch가 들어올 때마다 모델을 조금씩 업데이트합니다.

이 방식은 두 가지 상황에서 특히 유용합니다.

| 상황 | 이유 |
| :--- | :--- |
| 데이터가 너무 커서 batch retraining이 어려운 경우 | 전체 데이터를 한 번에 메모리에 올려 학습하기 어려움 |
| 외부 환경이 매우 빠르게 변하는 경우 | 모델이 실시간에 가깝게 변화에 적응해야 함 |

예를 들어 소셜미디어 트렌드는 하루 중에도 빠르게 변합니다. 이런 환경에서는 정해진 주기마다 재학습하는 것보다, 새 데이터가 들어올 때마다 모델을 조금씩 업데이트하는 것이 더 적합할 수 있습니다.

---

# 4. Model Versioning

## 핵심 개념

ML 모델 개발은 feature, algorithm, hyperparameter 조합을 계속 실험하는 반복 과정입니다. 또한 production에 배포한 후에도 재학습과 업데이트를 거치며 여러 버전의 모델이 생깁니다. 따라서 모델 버전을 제대로 관리하지 않으면 어떤 모델이 어떤 데이터와 pipeline, code에 의해 만들어졌는지 추적하기 어렵습니다.

ML system versioning은 일반 software versioning보다 복잡합니다. code만 versioning하면 부족합니다.

```text
Application Code Version
+ Model Version
+ Data Pipeline Version
+ Training Data Version
+ Dependency 관계
= ML System Versioning
```

예를 들어 어떤 application code가 특정 model version을 사용하고, 그 model version은 특정 data pipeline version과 특정 original data version에 의존할 수 있습니다. 따라서 ML 시스템에서는 model, data, pipeline, application을 함께 versioning하고, 서로의 dependency를 기록해야 합니다.

---

## Model Version에 기록해야 할 것

모델을 versioning한다는 것은 단순히 `model_v1.pkl`, `model_v2.pkl`처럼 파일 이름을 나누는 것이 아닙니다. 각 model version이 어떻게 만들어졌는지 충분히 기록해야 합니다.

| 기록 항목 | 설명 |
| :--- | :--- |
| Algorithm | 어떤 알고리즘을 사용했는가 |
| Features | 어떤 input feature를 사용했는가 |
| Architecture | 모델 구조는 무엇인가 |
| Hyperparameters | 어떤 hyperparameter 값을 사용했는가 |
| Training Data | 어떤 데이터 버전으로 학습했는가 |
| Data Pipeline | 어떤 pipeline version을 거쳤는가 |
| Dependencies | code, library, environment dependency |
| Performance | validation/test/production 성능은 어땠는가 |

---

## Model Versioning이 필요한 이유

Model versioning은 다음 목적을 위해 필요합니다.

| 이유 | 설명 |
| :--- | :--- |
| Performance Comparison | 여러 iteration의 성능을 비교할 수 있음 |
| Reproducibility | 같은 결과를 다시 만들 수 있음 |
| Collaboration | 여러 사람과 팀이 같은 버전을 기준으로 작업 가능 |
| Rollback | production 문제가 생기면 이전 안정 버전으로 되돌릴 수 있음 |
| Parallel Testing | 여러 모델을 병렬로 비교할 수 있음 |
| Production Governance | 어떤 모델이 실제 서비스 중인지 추적 가능 |

특히 production model에서 문제가 생겼을 때 rollback capability는 매우 중요합니다. 이전에 안정적으로 작동하던 모델 버전으로 빠르게 되돌릴 수 있어야 서비스 리스크를 줄일 수 있습니다.

---

## Shadow Release

Shadow release는 새 모델을 바로 production에 적용하지 않고, 기존 production model과 병렬로 실행해보는 방식입니다.

```text
Production Model
→ 실제 사용자에게 prediction 제공

Retrained / New Model
→ 같은 input으로 prediction 생성
→ 하지만 사용자에게는 노출하지 않음
→ performance만 logging

두 모델 성능 비교
→ 새 모델이 지속적으로 더 좋으면 production으로 전환
```

이 방식은 새 모델을 blind하게 배포하는 위험을 줄입니다. retrained model이 실제 production data에서 기존 모델보다 안정적으로 더 좋은 성능을 보이는지 확인한 뒤에 교체할 수 있습니다.

---

## Champion-Challenger Testing

Champion-challenger testing은 현재 production model을 champion으로 두고, 하나 이상의 challenger model을 병렬로 운영하며 비교하는 방식입니다.

```text
Champion Model
→ 현재 production model

Challenger Models
→ 다른 feature set
→ 다른 algorithm
→ 다른 training data
→ 다른 hyperparameter

성능 비교
→ challenger가 champion보다 지속적으로 우수
→ challenger를 새로운 champion으로 승격
```

이 방식은 model improvement를 체계적으로 관리하는 데 유용합니다. 모델을 한 번 배포하고 끝내는 것이 아니라, 여러 후보 모델을 계속 비교하면서 더 좋은 모델로 production model을 교체할 수 있습니다.

---

# 5. Organizational Considerations

## 핵심 개념

ML을 잘 모르는 관리자들은 모델이 production에 배포되면 데이터 사이언티스트와 ML 엔지니어의 일이 끝났다고 생각할 수 있습니다. 하지만 실제로는 배포 이후에도 상당한 지원과 운영 자원이 필요합니다.

모델 운영에는 다음과 같은 지속적인 책임이 따릅니다.

```text
monitoring system 운영
data drift / concept drift 감지
model maintenance 수행
retraining / updating 실행
model version management
shadow release 관리
champion-challenger testing 관리
customer service team 지원
user issue 대응 경로 마련
```

즉, ML 프로젝트 팀은 모델 배포 후 완전히 해산되면 안 됩니다. 최소한 일부 인력은 production model support에 계속 관여해야 합니다.

---

## Customer Service Team 지원

ML 모델이 제품 뒤에서 작동하는 경우, 사용자 문제를 처음 접하는 팀은 보통 customer service team입니다. 그런데 customer service team이 모델의 존재나 역할을 이해하지 못하면, 사용자 문의에 제대로 대응하기 어렵습니다.

따라서 프로젝트 팀은 customer service team이 다음을 이해하도록 도와야 합니다.

```text
제품 안에 ML 모델이 있다는 사실
모델이 어떤 역할을 하는지
모델 output이 어떤 의미인지
사용자가 왜 특정 prediction을 받았는지 설명하는 방법
모델 문제가 발생했을 때 어디로 escalate해야 하는지
```

가능한 범위에서 모델 예측을 설명할 수 있는 자료와 절차를 제공해야 합니다. 사용자가 “왜 이런 결과가 나왔나요?”라고 물었을 때, 완벽한 내부 수학 설명은 아니더라도 제품 수준에서 납득 가능한 설명이 있어야 합니다.

---

## Recourse Path

아무리 monitoring system이 좋아도 모델 문제는 발생할 수 있습니다. 따라서 사용자가 잘못된 prediction이나 이상한 output을 발견했을 때 따를 수 있는 recourse path가 필요합니다.

```text
사용자 문제 제기
→ customer service 접수
→ 모델 관련 issue인지 확인
→ ML/product team으로 escalation
→ 원인 분석
→ 필요 시 retraining, updating, rollback
→ 사용자 또는 고객에게 피드백
```

이런 경로가 없으면 모델 문제는 사용자 불만으로만 남고, 팀은 실제 production issue를 놓칠 수 있습니다.

---

# 6. 전체 흐름으로 보는 Model Lifecycle Management

Module 5의 운영 흐름을 하나로 보면 다음과 같습니다.

```text
Model Development
→ Model Versioning
→ Production Deployment
→ Monitoring
   - input data
   - pipeline output
   - model prediction
   - target label
   - subgroup performance
   - feature impact
→ Performance Decay Detection
→ Maintenance Decision
   - retraining
   - updating
   - rollback
→ Shadow Release / Champion-Challenger Test
→ New Model Deployment
→ Customer Support / Recourse
→ 다시 Monitoring
```

---

# 7. 실무적으로 중요한 인사이트

## 1. ML 모델은 배포 후 조용히 실패할 수 있다

모델이 틀린 prediction을 내고 있어도 사용자가 즉시 알아차리지 못할 수 있습니다. 그래서 production model에는 input, pipeline, output, label, subgroup 성능을 추적하는 monitoring 체계가 필요합니다.

## 2. Drift는 input만의 문제가 아니다

Data drift는 input distribution 변화이고, concept drift는 input과 target 사이의 관계 변화입니다. 모델 운영에서는 두 가지를 모두 봐야 합니다. input 분포가 변하지 않아도 target과의 관계가 바뀌면 모델은 틀릴 수 있습니다.

## 3. 전체 성능만 보면 bias를 놓칠 수 있다

전체 metric이 좋아도 특정 subgroup에서 성능이 낮으면 실제 서비스에서는 심각한 문제가 될 수 있습니다. production monitoring에서는 subgroup별 성능을 반드시 확인해야 합니다.

## 4. Retraining과 Updating은 다르다

Retraining은 기존 모델 구조를 유지하고 새 데이터로 weight를 다시 학습하는 것이고, updating은 알고리즘, hyperparameter, feature set까지 다시 검토하는 것입니다. 성능 저하의 원인에 따라 둘 중 무엇을 할지 결정해야 합니다.

## 5. Model versioning은 rollback과 실험 운영의 기반이다

Production에서 문제가 생겼을 때 이전 모델로 되돌릴 수 있어야 하고, 새 모델은 shadow release나 champion-challenger testing을 통해 안전하게 검증해야 합니다. 이를 위해서는 model뿐 아니라 data, pipeline, application code까지 함께 versioning해야 합니다.

## 6. 배포 후에도 팀 자원이 필요하다

관리자가 흔히 착각하는 부분은 “모델을 배포했으니 팀을 다른 프로젝트로 보내도 된다”는 것입니다. 실제로는 monitoring, retraining, updating, versioning, user support를 위해 지속적인 팀 참여가 필요합니다.

## 7. 고객 지원팀도 ML 제품의 일부다

사용자는 모델 문제가 생겼을 때 ML 엔지니어에게 바로 연락하지 않습니다. 대개 customer service team을 통해 문제를 제기합니다. 따라서 고객 지원팀이 모델의 역할과 예측 결과를 이해하고, 문제를 escalation할 수 있어야 합니다.

---

# 8. 프로젝트 / 면접에 써먹을 수 있는 문장

배포된 ML 모델은 시간이 지나며 data drift와 concept drift로 인해 성능이 저하될 수 있으므로, input data, pipeline output, model prediction, target label, subgroup performance를 지속적으로 모니터링해야 합니다. 또한 retraining과 updating 정책, model versioning, rollback, shadow release, champion-challenger testing을 통해 production model을 안정적으로 운영하는 lifecycle management 체계가 필요합니다.

---

# 최종 정리

Module 5의 핵심은 ML 모델이 배포 이후에도 계속 살아 있는 운영 대상이라는 점입니다. 모델은 현실 환경 변화, 데이터 분포 변화, 사용자 행동 변화, adversarial behavior 때문에 시간이 지나며 성능이 떨어질 수 있습니다. 따라서 production model은 단순히 서버에 올리는 것으로 끝나는 것이 아니라, 지속적으로 감시하고, 성능 저하를 감지하고, 새 데이터로 재학습하거나 모델 구조를 업데이트해야 합니다.

좋은 ML 운영 체계는 monitoring, maintenance, versioning, rollback, shadow release, champion-challenger testing, customer support까지 포함합니다. 결국 ML 프로젝트의 성공은 모델을 만드는 순간이 아니라, 모델이 실제 사용자 환경에서 오랫동안 신뢰할 수 있는 성능을 유지하도록 관리하는 데서 결정됩니다.