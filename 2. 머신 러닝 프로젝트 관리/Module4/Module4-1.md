# Managing Machine Learning Projects

## Module 4 — ML System Design & Technology Selection

### ML System Design Considerations / Cloud vs Edge / Online Learning & Inference / ML on Big Data

---

## 한 줄 요약

ML 시스템 설계는 단순히 모델을 고르는 문제가 아니라, **사용자가 어떤 방식으로 예측 결과를 받아야 하는지, 데이터가 어디서 오고 얼마나 빠르게 들어오는지, 모델을 어디서 학습·실행할지, 예측을 실시간으로 할지 배치로 할지**를 결정하는 문제입니다. 즉, 모델 성능뿐 아니라 latency, connectivity, privacy, data volume, throughput, infrastructure cost까지 함께 고려해야 합니다.

---

# 1. ML System Design Considerations

## ML 시스템의 4가지 구성 요소

ML 시스템은 모델 하나만으로 구성되지 않습니다. 강의에서는 ML 시스템을 네 가지 주요 구성 요소로 설명합니다.

| 구성 요소 | 의미 |
| :--- | :--- |
| User Interface / Application | 사용자가 모델의 출력을 소비하는 앱 또는 인터페이스 |
| Model | 모델 자체, 학습/재학습 방법, 예측 생성 방식 |
| Data / Pipeline | 데이터를 수집하고 처리해 모델 입력으로 만드는 파이프라인 |
| Infrastructure | 데이터, 모델, 앱이 동작하는 클라우드/엣지/서버 인프라 |

즉, ML 시스템 설계는 “어떤 알고리즘을 쓸까?”보다 넓은 문제입니다. 모델이 어디서 실행되는지, 데이터가 어떻게 들어오는지, 예측 결과가 얼마나 빨리 사용자에게 전달되어야 하는지에 따라 전체 기술 선택이 달라집니다.

---

## ML 시스템 설계에서 내려야 하는 핵심 결정

강의에서 다루는 주요 설계 결정은 크게 세 가지입니다.

| 설계 질문 | 선택지 | 핵심 기준 |
| :--- | :--- | :--- |
| 모델을 어디서 실행할 것인가? | Cloud vs Edge | latency, connectivity, privacy, compute |
| 모델을 어떻게 학습/재학습할 것인가? | Offline Learning vs Online Learning | 데이터 변화 속도, 구현 난이도, 성능 평가 방식 |
| 예측을 어떻게 제공할 것인가? | Batch Prediction vs Online Prediction | 실시간성, 처리 효율, latency 요구사항 |

이 선택들은 독립적으로 결정되는 것이 아니라, 사용자 요구사항과 제약 조건에 의해 결정됩니다.

```text
User Requirements / Constraints
→ Cloud or Edge
→ Offline or Online Learning
→ Batch or Online Prediction
→ Technology Selection
```

---

# 2. Cloud vs Edge

## Cloud ML

Cloud ML은 데이터가 클라우드로 전송되고, 모델 학습과 예측도 클라우드에서 수행되는 구조입니다. 결과는 다시 앱이나 사용자 기기로 전달됩니다.

Cloud 방식의 장점은 대량의 데이터와 높은 처리량을 다루기 좋다는 점입니다. 클라우드에서는 더 큰 저장 공간과 더 강한 컴퓨팅 자원을 활용할 수 있고, 수많은 사용자의 데이터를 기반으로 모델을 학습하거나 예측 결과를 생성하기 쉽습니다.

대표적인 예시는 영화 추천 시스템입니다. 추천 모델은 수백만~수십억 개의 사용자 행동 데이터를 기반으로 학습될 수 있고, 사용자는 보통 인터넷에 연결된 TV나 노트북으로 추천 결과를 확인합니다. 이 경우 연결성이 확보되어 있고, 대규모 데이터를 활용해야 하므로 cloud-based system이 적합합니다.

---

## Edge ML

Edge ML은 모델이 클라우드가 아니라 스마트폰, 센서, 카메라, 차량, 공장 장비 같은 edge device에서 직접 실행되는 구조입니다.

Edge 방식의 핵심 장점은 낮은 latency, 연결성 독립성, privacy입니다. 데이터를 클라우드로 보내고 다시 결과를 받는 시간이 줄어들기 때문에 즉각적인 예측이 가능하고, 인터넷이 없어도 작동할 수 있습니다. 또한 민감한 데이터를 기기 밖으로 보내지 않아도 됩니다.

대표적인 예시는 얼굴 인식으로 스마트폰 잠금을 해제하는 기능입니다. 사용자는 즉시 잠금이 풀리길 기대하고, 인터넷 연결이 없어도 작동해야 하며, 얼굴 이미지를 매번 클라우드로 보내는 것은 privacy 측면에서 부담이 큽니다. 따라서 이런 경우에는 edge-based system이 적합합니다.

---

## Cloud vs Edge 비교

| 기준 | Cloud ML | Edge ML |
| :--- | :--- | :--- |
| 실행 위치 | 클라우드 서버 | 사용자 기기, 센서, 스마트폰, 차량 등 |
| 핵심 장점 | 대규모 저장/처리, 높은 throughput | 낮은 latency, privacy, 오프라인 작동 |
| 주요 제약 | 인터넷 연결 필요, latency 발생 가능 | 기기의 compute, memory, power 제약 |
| 적합한 상황 | 대량 데이터 학습, 추천 시스템, 수요 예측 | 실시간 반응, 민감 데이터, 연결 불안정 환경 |
| 예시 | 영화 추천, 챗봇, 리테일 수요 예측 | 스마트폰 얼굴 인식, 제조 품질 검사, 자율주행 |

---

## Cloud가 적합한 경우

Cloud ML은 다음 조건에서 적합합니다.

| 조건 | 이유 |
| :--- | :--- |
| 사용자가 인터넷에 연결되어 있음 | 데이터를 클라우드로 보내고 결과를 받을 수 있음 |
| 대규모 historical data가 필요함 | 클라우드 저장소와 분산 처리 자원을 활용 가능 |
| 높은 throughput이 필요함 | 많은 사용자와 대량 데이터를 한 번에 처리 가능 |
| latency가 아주 치명적이지 않음 | 예측을 미리 만들어 캐싱할 수 있음 |

예를 들어 영화 추천 시스템은 매번 사용자가 요청할 때 모델을 새로 실행하지 않아도 됩니다. 매일 밤 또는 매시간 batch prediction을 실행해 사용자별 추천 결과를 미리 만들어두고, 사용자가 TV를 켰을 때 저장된 추천 결과를 빠르게 보여줄 수 있습니다.

---

## Edge가 적합한 경우

Edge ML은 다음 조건에서 적합합니다.

| 조건 | 이유 |
| :--- | :--- |
| latency가 매우 중요함 | 클라우드 왕복 시간을 기다릴 수 없음 |
| 인터넷 연결을 보장할 수 없음 | 기기 자체에서 예측해야 함 |
| privacy가 중요함 | 민감 데이터를 클라우드로 보내지 않아도 됨 |
| 즉각적인 판단이 필요함 | 공장, 차량, 보안 시스템 등에서 빠른 반응 필요 |

예를 들어 제조 라인의 품질 검사 시스템은 제품이 빠르게 이동하는 중에 바로 불량 여부를 판단해야 합니다. 데이터를 클라우드로 보냈다가 결과를 받는 구조는 너무 느릴 수 있습니다. 자율주행 차량도 마찬가지로 도로 상황을 즉시 판단해야 하므로 edge ML이 적합합니다.

---

## Hybrid Approach

Cloud와 Edge는 완전히 둘 중 하나만 선택하는 것이 아닙니다. 실제 시스템에서는 hybrid approach를 사용할 수 있습니다.

| 방식 | 설명 | 예시 |
| :--- | :--- | :--- |
| Edge trigger + Cloud processing | edge에서 특정 이벤트를 감지하면 cloud 분석을 시작 | 보안 카메라가 이상 행동 감지 후 cloud로 영상 전송 |
| Cloud prediction + Edge cache | cloud에서 자주 쓰는 예측값을 미리 계산해 기기에 저장 | 자주 쓰는 추천 결과를 기기에서 빠르게 조회 |
| Local data center 활용 | 사용자와 가까운 데이터센터로 요청을 보내 latency 감소 | 지역별 서버를 통한 빠른 응답 |

대표적인 hybrid 예시는 스마트 스피커입니다. 스피커 안의 edge model은 “Alexa” 같은 wake word를 감지합니다. wake word가 감지되면 이후 음성 명령은 cloud로 보내져 더 복잡한 모델이 처리합니다.

```text
Edge Model
→ Wake word 감지
→ Cloud 연결 시작
→ 이후 음성 명령 처리
→ 결과 반환
```

---

## Cloud vs Edge 선택 질문

Cloud와 Edge를 선택할 때는 다음 질문을 던지면 됩니다.

```text
1. 사용자가 결과를 즉시 받아야 하는가?
2. 인터넷 연결이 항상 가능하다고 가정할 수 있는가?
3. 사용자가 데이터를 클라우드로 보내는 것을 받아들일 수 있는가?
4. 기기 자체에 모델을 실행할 compute, memory, power가 있는가?
5. 대규모 historical data를 이용한 학습이 필요한가?
```

latency, connectivity, privacy가 중요하면 edge 쪽으로 기울고, 대규모 데이터와 높은 처리량이 중요하며 연결성을 가정할 수 있으면 cloud 쪽으로 기웁니다.

---

# 3. Offline Learning vs Online Learning

## 핵심 개념

모델 학습과 재학습을 고정된 일정에 따라 할지, 새로운 데이터가 들어올 때마다 계속 업데이트할지에 따라 offline learning과 online learning으로 나뉩니다.

| 구분 | 의미 |
| :--- | :--- |
| Offline Learning | 정해진 주기마다 전체 또는 대량의 historical data로 재학습 |
| Online Learning | 새 데이터가 들어올 때마다 모델을 점진적으로 업데이트 |

---

## Offline Learning

Offline learning은 모델을 일정한 주기마다 재학습하는 방식입니다. 보통 몇 주 또는 몇 달 단위로 새 데이터를 모은 뒤, 전체 historical data를 사용해 다시 학습합니다.

Offline learning의 장점은 production 구현이 비교적 쉽고, 성능 평가도 쉽다는 점입니다. 전체 데이터셋을 기준으로 train/test split을 만들 수 있고, 모델 버전별 성능을 비교하기도 수월합니다.

단점은 환경 변화에 느리게 반응한다는 것입니다. 데이터 분포가 빠르게 바뀌는 문제에서는 모델이 현실 변화를 따라잡는 데 시간이 걸릴 수 있습니다.

```text
Offline Learning
→ 주기적으로 데이터 모음
→ 전체 historical data로 재학습
→ 평가 후 배포
```

현재 많은 ML 시스템은 offline learning을 사용합니다.

---

## Online Learning

Online learning은 새로운 데이터가 들어올 때마다 모델을 계속 업데이트하는 방식입니다. 보통 개별 데이터 포인트 또는 작은 batch 단위로 모델을 갱신합니다.

Online learning의 장점은 빠르게 변하는 환경에 잘 적응할 수 있다는 점입니다. 데이터 분포가 자주 바뀌거나, 패턴이 계속 변화하는 문제에서는 online learning이 유리합니다. 또한 전체 데이터가 너무 커서 매번 메모리에 올려 학습하기 어려운 big data 상황에서도 유용할 수 있습니다.

단점은 production 구현이 어렵고, 매번 모델 성능을 안정적으로 평가하기가 더 어렵다는 점입니다.

```text
Online Learning
→ 새 데이터 도착
→ 모델 즉시 또는 작은 batch 단위로 업데이트
→ 변화하는 데이터 분포에 빠르게 적응
```

---

## Offline Learning vs Online Learning 비교

| 기준 | Offline Learning | Online Learning |
| :--- | :--- | :--- |
| 학습 방식 | 정해진 주기마다 재학습 | 새 데이터가 들어올 때마다 업데이트 |
| 데이터 사용 | 전체 historical data 사용 가능 | 개별 데이터 또는 small batch 사용 |
| 구현 난이도 | 상대적으로 쉬움 | 더 어려움 |
| 성능 평가 | 비교적 쉬움 | 더 어려움 |
| 변화 대응 | 느림 | 빠름 |
| Big Data 대응 | 데이터가 너무 크면 부담 | 전체 데이터를 메모리에 올리지 않아도 됨 |
| 예시 | 추천 모델 주기적 재학습 | 스팸 탐지, 빠르게 변하는 뉴스 추천 |

---

## Online Learning이 필요한 예시

### 1. Social Media Spam Detection

소셜미디어 스팸은 매우 빠르게 형태가 바뀝니다. 스팸을 만드는 사람들은 모델의 변화를 보고 새로운 방식으로 회피하려고 하기 때문에, 모델도 계속 업데이트되어야 합니다. 이런 경우 online learning은 변화하는 데이터 분포에 빠르게 적응하는 데 도움이 됩니다.

### 2. News Recommendation

사용자가 평소에는 스포츠 기사를 많이 읽더라도, 특정 정치적 사건이 크게 발생한 날에는 그 사건 관련 기사를 더 보고 싶어 할 수 있습니다. Offline learning 기반 추천 모델은 과거 스포츠 선호를 계속 반영할 수 있지만, online learning 시스템은 사용자의 당일 행동을 빠르게 반영해 추천을 조정할 수 있습니다.

---

# 4. Batch Prediction vs Online Prediction

## 핵심 개념

학습 방식과 별개로, 예측 결과를 어떻게 생성할지도 결정해야 합니다. 정해진 주기마다 여러 데이터에 대해 예측을 만들어두면 batch prediction이고, 요청이 들어올 때마다 즉시 예측하면 online prediction입니다.

| 구분 | 의미 |
| :--- | :--- |
| Batch Prediction | 일정 주기마다 데이터 묶음에 대해 예측 생성 |
| Online Prediction | 요청이 들어올 때마다 실시간 예측 생성 |

---

## Batch Prediction

Batch prediction은 여러 데이터 포인트를 모아서 한 번에 예측하는 방식입니다. 예를 들어 매일 밤 모든 사용자에 대한 추천 결과를 미리 계산하고 저장해둘 수 있습니다.

장점은 계산 효율이 좋고, 대량 데이터 처리에 적합하며, 데이터 drift를 모니터링하기 더 쉽다는 점입니다. 단점은 새로운 데이터에 대한 예측이 즉시 제공되지 않는다는 것입니다. 다음 batch가 실행될 때까지 기다려야 합니다.

```text
Batch Prediction
→ 데이터 누적
→ 일정 시간마다 모델 실행
→ 예측 결과 저장
→ 사용자는 저장된 결과 조회
```

추천 시스템, 수요 예측, 정기 리포트 생성처럼 실시간성이 덜 중요한 문제에 적합합니다.

---

## Online Prediction

Online prediction은 사용자가 요청을 보내거나 새로운 데이터가 들어올 때마다 모델이 즉시 예측을 생성하는 방식입니다.

장점은 latency를 줄이고, 사용자에게 즉각적인 결과를 제공할 수 있다는 점입니다. 단점은 예측 요청마다 빠르게 응답해야 하므로 시스템 설계가 더 어렵고, drift 모니터링이나 안정적 운영도 더 까다로울 수 있습니다.

```text
Online Prediction
→ 사용자 요청 또는 새 데이터 도착
→ 모델 즉시 실행
→ 예측 결과 즉시 반환
```

실시간 번역 앱, 자율주행, 음식 배달 도착 시간 예측처럼 사용자가 즉시 결과를 기대하는 문제에 적합합니다.

---

## Batch Prediction vs Online Prediction 비교

| 기준 | Batch Prediction | Online Prediction |
| :--- | :--- | :--- |
| 예측 생성 시점 | 고정된 주기 | 요청 발생 즉시 |
| 장점 | 계산 효율 좋음, 대량 처리에 유리 | latency 낮음, 즉시 응답 가능 |
| 단점 | 새 데이터에 대한 즉시 예측 불가 | 구현/운영 난이도 높음 |
| 적합한 상황 | 추천 결과 사전 계산, 수요 예측 | 번역, 자율주행, 배달 ETA |
| 핵심 질문 | 미리 계산해둬도 되는가? | 지금 바로 답을 줘야 하는가? |

---

# 5. 예시로 보는 시스템 설계 선택

## 스마트폰 얼굴 인식 잠금 해제

스마트폰 얼굴 인식 시스템은 사용자가 즉시 결과를 기대하고, 인터넷 연결이 없을 수도 있으며, 얼굴 이미지는 민감한 개인정보입니다.

따라서 이 시스템은 edge-based system이 적합합니다. 모델은 기기 안에서 실행되고, 학습은 초기 설정 시 또는 일정 주기마다 offline learning 방식으로 수행될 수 있으며, 예측은 사용자가 휴대폰을 들 때마다 online prediction으로 생성됩니다.

```text
Use Case: Face Unlock

요구사항:
- 매우 낮은 latency
- 인터넷 연결 불필요
- 얼굴 이미지 privacy 보호

설계:
- Edge system
- Offline learning
- Online prediction
```

---

## 영화 추천 시스템

영화 추천 시스템은 수많은 사용자 기록과 평점 데이터를 활용합니다. 사용자는 보통 인터넷에 연결된 TV나 노트북으로 추천을 받습니다. 모델 학습에는 대규모 historical data가 필요하고, 추천 결과는 미리 계산해 저장해둘 수 있습니다.

따라서 cloud-based system, offline learning, batch prediction이 적합합니다.

```text
Use Case: Movie Recommendation

요구사항:
- 대규모 사용자 데이터 활용
- 인터넷 연결 가능
- 높은 throughput
- 추천 결과를 미리 계산 가능

설계:
- Cloud system
- Offline learning
- Batch prediction
```

---

## 실시간 번역 앱

번역 앱은 사용자가 문장을 입력하거나 말하면 즉시 결과를 기대합니다. 따라서 prediction은 online prediction에 가깝습니다. 다만 모델 자체는 클라우드에서 실행될 수도 있고, 기기 성능과 privacy 요구에 따라 edge 또는 hybrid 구조도 가능합니다.

```text
Use Case: Real-time Translation

요구사항:
- 즉각적인 예측 결과
- 낮은 latency
- 사용자 요청마다 다른 입력

설계:
- Online prediction
- Cloud / Edge / Hybrid 가능
```

---

## 음식 배달 ETA 예측

음식 배달 서비스에서 사용자는 주문 직후 예상 도착 시간을 알고 싶어 합니다. 다음 batch까지 기다리게 할 수 없기 때문에 online prediction이 적합합니다.

```text
Use Case: Food Delivery ETA

요구사항:
- 주문 시점에 즉시 예측 필요
- 현재 위치, 교통, 음식 준비 상황 등 반영
- 사용자별 요청마다 예측 생성

설계:
- Online prediction
```

---

# 6. ML on Big Data

## 핵심 개념

Big Data는 ML 시스템 설계에 큰 영향을 줍니다. 여기서 중요한 것은 데이터의 **volume**뿐 아니라, 데이터가 시스템을 통과하는 **velocity**입니다.

```text
Volume
→ 데이터 양이 매우 큼

Velocity
→ 데이터가 매우 빠른 속도로 계속 들어옴
```

예를 들어 수천~수백만 개의 센서에서 고빈도 데이터가 생성되거나, 소셜미디어에서 수백만~수십억 사용자의 데이터가 하루 종일 계속 업데이트되는 상황은 big data 문제에 해당합니다.

의료 분야에서도 수백만 명의 환자에 대해 전자건강기록, 센서 데이터, 치료 기록, 의료 이미지가 장기간 쌓이면 big data 문제가 됩니다.

---

## Big Data가 만드는 시스템 설계 문제

Big Data는 ML 시스템에서 세 가지 주요 문제를 만듭니다.

| 문제 | 설명 |
| :--- | :--- |
| Storage | 방대한 데이터를 저장하고 관리해야 함 |
| Processing | 데이터를 처리하고 모델 학습에 사용하는 compute 비용이 큼 |
| Modeling | 전체 데이터를 메모리에 올려 학습하기 어렵고 학습 시간이 길어짐 |

작은 데이터셋에서는 scatter plot이나 correlation matrix 같은 시각화로 데이터 패턴, 품질 문제, missing data, outlier를 확인하기 쉽습니다. 하지만 big data에서는 전체를 단순 시각화로 이해하는 것이 어려울 수 있습니다. 따라서 sampling, distributed processing, summary statistics, automated data quality checks 같은 다른 접근이 필요해집니다.

---

## Big Data가 모델링에 주는 영향

데이터가 너무 커서 메모리에 올릴 수 없으면 일반적인 방식으로 모델을 학습하기 어렵습니다. 이때는 분산 처리 기술이나 online learning 같은 다른 학습 전략이 필요할 수 있습니다.

또한 데이터 규모가 크면 모델 학습 주기가 길어지고, 경우에 따라 예측 생성에도 latency가 생길 수 있습니다. 따라서 Big Data 환경에서는 모델 성능뿐 아니라 학습 시간, 처리 비용, 예측 latency까지 함께 고려해야 합니다.

```text
Big Data
→ memory에 전체 데이터 적재 불가
→ distributed storage / distributed processing 필요
→ training cycle 증가
→ online learning 고려
→ prediction latency 관리 필요
```

---

## Big Data 환경에서 필요한 인프라

Big Data를 다루는 ML 시스템은 보통 다음 요소가 필요합니다.

| 요소 | 설명 |
| :--- | :--- |
| Distributed Storage | 대규모 데이터를 여러 저장소에 분산 저장 |
| Cloud-based Storage | 클라우드 기반 저장소로 확장성 확보 |
| Distributed Processing | 대규모 데이터 파이프라인과 모델 학습을 분산 처리 |
| Scalable Data Pipeline | 대량 데이터 수집, 정제, 변환을 안정적으로 수행 |
| Online Learning | 전체 데이터를 매번 학습하기 어려울 때 새 데이터로 점진 업데이트 |

Big Data에서 핵심은 “더 큰 모델을 쓰자”가 아니라, 데이터 규모와 속도를 감당할 수 있는 storage, processing, pipeline, learning strategy를 함께 설계하는 것입니다.

---

# 7. 설계 판단 기준 요약

Module 4의 시스템 설계 판단을 하나의 흐름으로 보면 다음과 같습니다.

```text
사용자 요구사항 확인
→ latency가 중요한가?
→ 인터넷 연결을 가정할 수 있는가?
→ privacy가 중요한가?
→ 데이터 규모와 속도는 어느 정도인가?
→ 모델을 얼마나 자주 재학습해야 하는가?
→ 예측을 미리 만들어도 되는가, 즉시 생성해야 하는가?
→ cloud / edge / hybrid 선택
→ offline / online learning 선택
→ batch / online prediction 선택
→ 필요한 storage / processing / deployment 기술 선택
```

---

# 8. 실무적으로 중요한 인사이트

## 1. ML 시스템 설계는 모델 선택보다 넓다

ML 시스템은 UI, 모델, 데이터 파이프라인, 인프라가 함께 작동하는 구조입니다. 모델 성능이 좋아도 예측이 너무 느리거나, 데이터가 제때 들어오지 않거나, 사용자가 결과를 받아볼 수 없다면 좋은 시스템이 아닙니다.

## 2. Cloud와 Edge의 선택은 기술 취향이 아니라 제약 조건의 문제다

latency, connectivity, privacy가 중요하면 edge가 유리하고, 대규모 데이터와 높은 throughput이 중요하면 cloud가 유리합니다. 많은 실제 시스템은 둘을 섞은 hybrid 구조를 사용합니다.

## 3. Learning과 Prediction은 구분해야 한다

offline/online learning은 모델을 언제 어떻게 업데이트할지에 대한 문제이고, batch/online prediction은 예측 결과를 언제 어떻게 생성할지에 대한 문제입니다. 둘은 서로 다른 설계 축입니다.

## 4. Online learning은 강력하지만 운영이 어렵다

빠르게 변하는 데이터 분포에 적응하기 좋지만, production 구현과 성능 평가가 더 어렵습니다. 따라서 정말 빠른 적응이 필요한 문제인지 먼저 판단해야 합니다.

## 5. Batch prediction은 느린 방식이 아니라 효율적인 방식이다

추천 시스템처럼 예측을 미리 만들어도 되는 경우에는 batch prediction이 훨씬 효율적입니다. 사용자는 즉시 결과를 보는 것처럼 느끼지만, 실제 예측은 미리 계산되어 cache되어 있을 수 있습니다.

## 6. Big Data는 저장 문제가 아니라 시스템 설계 문제다

Big Data는 저장 공간만 많이 필요한 것이 아닙니다. 데이터 탐색, 품질 확인, 파이프라인 처리, 모델 학습, 예측 latency, 비용까지 모두 바꿉니다. 따라서 Big Data 환경에서는 분산 저장, 분산 처리, cloud infrastructure, online learning 같은 설계 선택이 중요해집니다.

---

# 9. 프로젝트 / 면접에 써먹을 수 있는 문장

ML 시스템 설계에서는 모델 자체뿐 아니라 사용자 인터페이스, 데이터 파이프라인, 인프라, 학습 방식, 예측 제공 방식을 함께 고려해야 합니다. latency, connectivity, privacy, data volume, data velocity 같은 제약 조건에 따라 cloud, edge, hybrid 구조를 선택하고, offline/online learning과 batch/online prediction을 구분해 설계하는 것이 중요합니다.

---

# 최종 정리

Module 4의 핵심은 ML 시스템을 “모델을 학습시키는 코드”가 아니라 “사용자가 실제로 예측 결과를 얻는 전체 시스템”으로 봐야 한다는 것입니다. 얼굴 인식처럼 즉각적이고 privacy가 중요한 문제는 edge system이 적합하고, 영화 추천처럼 대규모 데이터를 활용하고 미리 예측 결과를 계산할 수 있는 문제는 cloud system과 batch prediction이 적합합니다.

또한 모델을 주기적으로 재학습할지, 새 데이터가 들어올 때마다 갱신할지, 예측을 미리 계산할지, 요청 시점에 즉시 계산할지에 따라 시스템 복잡도와 기술 스택이 달라집니다. Big Data 환경에서는 데이터의 양과 속도가 storage, processing, training, inference 방식을 모두 바꾸기 때문에, ML 시스템 설계는 데이터 규모와 운영 제약까지 함께 고려해야 합니다.