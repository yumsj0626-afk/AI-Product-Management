
# Managing Machine Learning Projects

## Module 4 — ML System Design & Technology Selection

### ML Technology Selection / Common ML Tools

---

## 한 줄 요약

ML 기술 선택은 “요즘 많이 쓰는 도구를 고르는 일”이 아니라, **시스템 설계 결정과 팀의 역량, 비용, 유지보수성, 모델 유형, 배포 방식에 맞는 기술 조합을 고르는 일**입니다. Cloud/Edge, Offline/Online, Batch/Online Prediction 같은 설계 방향을 먼저 정한 뒤, 프로그래밍 언어, 데이터 파이프라인 도구, 모델링 라이브러리, API/배포 기술을 선택해야 합니다.

---

# 1. ML Technology Selection

## 핵심 개념

사용자 요구사항을 이해하고 ML 시스템 설계 결정을 내린 뒤에는, 실제 프로젝트에서 사용할 기술을 선택해야 합니다. 이때 기술 선택은 단순히 모델링 라이브러리만 고르는 것이 아니라, 전체 ML 시스템을 구성하는 여러 영역의 기술을 함께 고르는 일입니다.

```text
User Requirements
→ System Design Decisions
   - Cloud vs Edge
   - Offline vs Online Learning
   - Batch vs Online Prediction
→ Technology Selection
   - Programming Language
   - Data Pipeline Tools
   - Modeling Tools
   - API / Interface Technologies
```

---

## ML 프로젝트에서 선택해야 하는 기술 영역

| 영역 | 설명 |
| :--- | :--- |
| Programming Language | ML 코드와 데이터 처리 코드를 작성할 언어 선택 |
| Data Pipeline / Processing Tools | 데이터 수집, 정제, feature 생성, 변환을 위한 도구 선택 |
| Modeling Tools | 모델 학습, 평가, 튜닝에 사용할 라이브러리나 플랫폼 선택 |
| API / Interface Technologies | 모델과 애플리케이션을 연결하는 인터페이스 기술 선택 |
| Infrastructure | 모델 학습과 추론이 실행될 클라우드, 서버, 엣지 환경 선택 |

특히 모델 유형에 따라 필요한 도구가 달라집니다. 전통적인 ML 모델을 만들 때와 deep learning neural network를 만들 때 사용하는 도구는 다를 수 있습니다. 또한 모델을 cloud에서 돌릴지, edge device에서 돌릴지에 따라서도 기술 선택이 달라집니다.

---

# 2. 기술 선택 시 고려해야 할 기준

기술을 고를 때는 단순히 “성능이 좋은가?”만 보면 안 됩니다. 실제 프로젝트에서는 비용, 학습 난이도, 문서화, 커뮤니티, 채용 가능성, 유연성, 유지보수 비용까지 함께 고려해야 합니다.

| 기준 | 핵심 질문 |
| :--- | :--- |
| Open Source vs Proprietary | 무료/개방형 도구를 쓸 것인가, 상용 도구를 쓸 것인가? |
| Cost | 초기 비용뿐 아니라 장기 운영과 지원 비용은 어떤가? |
| Learning Curve | 팀원이 얼마나 빠르게 익힐 수 있는가? |
| Documentation | 공식 문서, 튜토리얼, 강의, 예제가 충분한가? |
| Community Support | 질문을 해결할 커뮤니티와 포럼이 활성화되어 있는가? |
| Talent Availability | 해당 도구를 다룰 수 있는 인력을 채용하기 쉬운가? |
| Flexibility vs Simplicity | 유연하지만 복잡한 도구인가, 단순하지만 제한적인 도구인가? |
| Lifetime Cost | 개발, 운영, 유지보수, 재학습까지 포함한 전체 비용은 어떤가? |

---

## Open Source vs Proprietary

Open source 도구는 보통 비용이 낮고 커뮤니티가 크며, 인재를 찾기 쉽다는 장점이 있습니다. 반면 공식 지원이나 기업용 기능은 제한적일 수 있습니다.

Proprietary 또는 commercial 도구는 비용이 들지만, 기술 지원, 안정성, 통합 기능, 관리 기능을 제공할 수 있습니다. 특히 대기업이나 규제가 강한 산업에서는 공식 지원과 관리 기능이 중요할 수 있습니다.

---

## Learning Curve와 Documentation

새로운 도구를 선택할 때는 팀이 얼마나 빠르게 익힐 수 있는지도 중요합니다. 아무리 강력한 도구라도 학습 난이도가 너무 높고 문서가 부족하면 실제 프로젝트 속도를 늦출 수 있습니다.

좋은 도구는 보통 다음 조건을 가집니다.

```text
공식 문서가 잘 되어 있음
온라인 튜토리얼과 예제가 많음
관련 강의나 책이 있음
질문을 해결할 커뮤니티가 있음
```

---

## Community와 Talent Pool

도구의 사용자 커뮤니티가 크면 문제를 해결하기 쉽고, 새로운 팀원을 채용하기도 쉽습니다. Python처럼 사용자 기반이 큰 기술은 관련 자료와 개발자 풀이 풍부합니다. 반대로 niche한 도구를 선택하면 단기적으로는 편할 수 있어도, 장기적으로 인력 확보와 유지보수에서 어려움이 생길 수 있습니다.

---

## Flexibility vs Simplicity

기술 선택에는 보통 유연성과 단순성 사이의 trade-off가 있습니다.

```text
유연하고 커스터마이징 가능한 도구
→ 다양한 상황에 맞게 조정 가능
→ 대신 복잡하고 학습 비용이 높을 수 있음

단순하고 사용하기 쉬운 도구
→ 빠르게 시작 가능
→ 대신 특수한 use case에서는 제약이 있을 수 있음
```

따라서 팀의 역량과 프로젝트 요구사항에 맞춰 선택해야 합니다.

---

# 3. 모델링 도구 선택 옵션

모델링 도구를 선택할 때는 비용과 구현 시간을 함께 봐야 합니다. 강의에서는 크게 네 가지 선택지를 설명합니다.

| 선택지 | 비용 | 구현 시간 | 특징 |
| :--- | :--- | :--- | :--- |
| Build from Scratch | 낮음 | 김 | Python 등으로 알고리즘을 직접 구현 |
| Open Source ML Libraries | 낮음 | 짧아짐 | scikit-learn, TensorFlow, PyTorch 등 활용 |
| Commercial ML Libraries | 높음 | 짧아짐 | 상용 기능과 지원을 활용 |
| AutoML | 높을 수 있음 | 매우 짧아짐 | feature, algorithm, hyperparameter 선택 자동화 |

---

## 1. Build from Scratch

순수 Python 같은 언어로 모델과 알고리즘을 직접 구현하는 방식입니다. 도구 비용은 거의 들지 않지만, 구현 시간이 오래 걸립니다.

이 방식은 학습 목적이나 매우 특수한 알고리즘을 직접 구현해야 하는 경우에는 의미가 있지만, 일반적인 제품 개발에서는 시간이 많이 걸리고 유지보수 부담도 큽니다.

---

## 2. Open Source ML Libraries

기존 open source ML library를 사용하는 방식입니다. 비용이 낮고 구현 속도를 크게 줄일 수 있습니다.

대표적인 예시는 다음과 같습니다.

```text
scikit-learn
TensorFlow
PyTorch
Keras
Pandas
NumPy
```

대부분의 ML 프로젝트는 처음부터 직접 알고리즘을 구현하기보다, 검증된 open source library를 활용하는 것이 현실적입니다.

---

## 3. Commercial ML Libraries

상용 ML 라이브러리나 플랫폼을 사용하는 방식입니다. 비용은 들지만, 구현 속도, 기술 지원, 기업용 기능, 관리 기능에서 장점이 있을 수 있습니다.

특히 조직 차원의 보안, 권한 관리, 모니터링, 협업 기능이 중요한 경우에는 상용 솔루션을 고려할 수 있습니다.

---

## 4. AutoML

AutoML은 모델 개발 과정의 일부 또는 대부분을 자동화하는 도구입니다. feature 선택, algorithm 선택, hyperparameter tuning 등을 자동으로 수행하여 빠르게 모델을 만들 수 있게 합니다.

AutoML은 ML 전문성이 부족한 팀이나 빠른 prototype이 필요한 상황에서 유용합니다. 하지만 비용이 발생할 수 있고, 모델 선택 과정이 black-box처럼 느껴질 수 있으며, 세밀한 통제가 필요한 프로젝트에서는 한계가 있을 수 있습니다.

---

# 4. Common Programming Languages

## Python

Python은 현재 데이터 과학과 머신러닝에서 가장 널리 쓰이는 언어입니다. 배우기 쉽고, 코드가 읽기 쉬우며, ML과 데이터 분석을 위한 library ecosystem이 매우 풍부합니다.

Python의 장점은 다음과 같습니다.

```text
배우기 쉽고 readable함
데이터 과학 / ML library가 풍부함
튜토리얼, 문서, 강의 등 학습 자료가 많음
커뮤니티가 큼
Python 개발자와 ML 인재를 찾기 쉬움
```

Python은 모델링, 데이터 처리, 실험, 시각화, API 개발까지 폭넓게 쓰일 수 있습니다.

---

## R

R은 통계 분석에 뿌리를 둔 언어입니다. 데이터 분석, 통계 모델링, 시각화에 강점이 있습니다.

R의 특징은 다음과 같습니다.

```text
통계 분석에 강함
그래프와 시각화 기능이 뛰어남
데이터 분석 library가 잘 발달되어 있음
Python보다는 커뮤니티와 범용성이 작음
```

R은 특히 통계 분석이나 연구 중심 데이터 분석에서 강점을 가질 수 있습니다.

---

## C / C++

C와 C++은 속도가 빠르고, 전통적인 소프트웨어 개발에서 많이 쓰입니다. Python이나 R보다 학습 난이도는 높지만, 성능이 중요한 시스템에서는 여전히 중요합니다.

ML 자체를 실험하고 분석하는 언어로는 Python보다 덜 쓰이지만, ML 모델이 들어가는 실제 소프트웨어나 성능이 중요한 시스템에서는 C/C++이 사용될 수 있습니다.

```text
장점
→ 빠른 실행 속도
→ 성능이 중요한 시스템에 적합

단점
→ 학습 난이도 높음
→ 데이터 과학 / ML ecosystem은 Python보다 약함
```

---

# 5. Jupyter Notebooks

## 핵심 개념

Jupyter Notebook은 데이터 과학자와 ML 실무자들이 많이 사용하는 개발 환경입니다. 전통적인 IDE와 달리 code, text, image, chart, output을 하나의 notebook 안에 함께 넣을 수 있습니다.

Jupyter가 인기 있는 이유는 ML 작업이 실험적이고 반복적인 성격을 가지기 때문입니다.

---

## Jupyter Notebook의 장점

| 장점 | 설명 |
| :--- | :--- |
| 실험에 적합 | cell 단위로 코드를 실행하며 빠르게 실험 가능 |
| 코드와 설명 결합 | 코드 사이에 텍스트, 이미지, 그래프, 다이어그램 삽입 가능 |
| 결과 즉시 확인 | 각 cell 실행 결과를 바로 확인 가능 |
| 문서화에 유리 | 분석 과정과 reasoning을 함께 기록하기 좋음 |
| 여러 언어 지원 | Python뿐 아니라 다양한 언어를 사용할 수 있음 |

Jupyter Notebook은 EDA, prototype modeling, 학습용 실험, 결과 공유에 매우 적합합니다.

---

## Jupyter Notebook의 단점

하지만 Jupyter Notebook은 production software engineering 관점에서는 약점이 있습니다.

| 단점 | 설명 |
| :--- | :--- |
| 테스트 어려움 | 일반적인 unit test 구조를 적용하기 어려움 |
| 버전 관리 어려움 | notebook 파일은 변경 내역 비교가 깔끔하지 않을 수 있음 |
| 실행 순서 문제 | cell 실행 순서에 따라 결과가 달라질 수 있음 |
| production 코드와 거리 | 실험용 코드를 그대로 서비스 코드로 쓰기 어려움 |

따라서 Jupyter는 탐색과 실험에는 좋지만, 실제 배포 시스템에서는 script, package, pipeline 구조로 정리하는 과정이 필요합니다.

---

# 6. Python ML Ecosystem

## Pandas와 NumPy

Pandas는 데이터 조작과 분석을 위한 대표적인 Python library입니다. 핵심 자료구조인 DataFrame은 Excel spreadsheet나 table과 비슷한 형태로 데이터를 다룰 수 있게 해줍니다.

Pandas로 할 수 있는 일은 다음과 같습니다.

```text
dataset 병합
데이터 조작
데이터 정제
기초 분석
table 형태 데이터 처리
```

Pandas는 NumPy 위에 만들어졌습니다. NumPy는 numerical array를 다루는 데 특화된 library입니다.

---

## Scikit-learn

Scikit-learn은 전통적인 ML 모델링에 많이 쓰이는 library입니다. 데이터 분석과 모델 학습을 위한 다양한 알고리즘을 제공하고, API가 일관적이고 사용하기 쉽습니다.

Scikit-learn의 장점은 다음과 같습니다.

```text
사용하기 쉬운 API
다양한 ML 알고리즘 제공
문서화가 잘 되어 있음
학습 곡선이 비교적 낮음
몇 줄의 코드로 모델 학습과 예측 가능
```

회귀, 분류, clustering, model selection, preprocessing 등 기본 ML 작업에 매우 자주 사용됩니다.

---

## Matplotlib

Matplotlib은 Python의 대표적인 시각화 library입니다. Pandas DataFrame이나 NumPy array에서 나온 데이터를 다양한 그래프로 시각화할 수 있습니다.

EDA, 모델 결과 분석, 보고서 작성, 실험 결과 시각화에 자주 사용됩니다.

---

## NLTK와 spaCy

텍스트 데이터를 다룰 때는 NLTK나 spaCy 같은 NLP library를 사용할 수 있습니다. 텍스트는 그대로 모델에 넣을 수 없기 때문에, 숫자 feature로 변환하는 전처리 과정이 필요합니다.

```text
Text Data
→ Tokenization
→ Cleaning / Preprocessing
→ Numerical Features
→ ML Model
```

NLTK와 spaCy는 이런 text preprocessing과 NLP 작업을 지원합니다.

---

# 7. Deep Learning Libraries

## TensorFlow, PyTorch, Keras

Deep learning model을 만들 때는 TensorFlow, PyTorch, Keras 같은 library가 많이 사용됩니다. 세 도구 모두 Python 기반 open source library이며, deep neural network를 만들고 학습시키는 데 사용됩니다.

공통적으로 CPU, GPU, TPU를 활용해 학습을 가속할 수 있고, 학습된 모델을 server나 edge device로 export해서 prediction에 사용할 수 있습니다.

| 도구 | 특징 |
| :--- | :--- |
| TensorFlow | Google이 개발·유지, 성숙한 ecosystem, 과거부터 큰 커뮤니티 보유 |
| PyTorch | Facebook AI Research가 개발·유지, 상대적으로 배우기 쉽고 인기가 빠르게 증가 |
| Keras | TensorFlow 위에서 동작하는 high-level API, 빠른 모델 개발과 실험에 적합 |

---

## TensorFlow

TensorFlow는 Google이 개발한 deep learning library입니다. 비교적 일찍 출시되어 성숙도가 높고, historically 큰 사용자 커뮤니티를 가지고 있습니다.

대규모 production 환경이나 Google ecosystem과 연결된 프로젝트에서 자주 고려될 수 있습니다.

---

## PyTorch

PyTorch는 Facebook AI Research에서 개발한 deep learning library입니다. TensorFlow보다 늦게 등장했지만, 사용하기 쉽고 직관적이라는 평가를 받으며 빠르게 성장했습니다.

연구, 실험, prototype 개발에서 특히 많이 사용되고 있으며, 최근에는 production 환경에서도 널리 쓰이고 있습니다.

---

## Keras

Keras는 TensorFlow 위에서 동작하는 high-level deep learning API입니다. 목적은 model building process를 단순화하고, 빠른 반복 실험을 가능하게 하는 것입니다.

복잡한 세부 구현보다 빠르게 모델을 만들고 평가하고 싶은 경우 유용합니다.

---

## Deep Learning Library 선택 기준

TensorFlow, PyTorch, Keras 사이에 절대적인 정답은 없습니다. 대부분의 경우 자신과 팀이 가장 익숙하고, 프로젝트 요구사항에 맞는 도구를 쓰면 됩니다.

```text
팀이 이미 익숙한가?
필요한 모델을 구현하기 쉬운가?
production 배포가 편한가?
문서와 예제가 충분한가?
커뮤니티가 활발한가?
기존 시스템과 잘 통합되는가?
```

---

# 8. AutoML

## 핵심 개념

AutoML은 feature 선택, algorithm 선택, hyperparameter tuning 등 모델 개발 과정의 일부를 자동화하는 도구입니다. 제한된 ML 전문성으로도 빠르게 모델을 만들 수 있게 해주며, no-code 또는 low-code 방식으로 제공되는 경우도 많습니다.

대표적인 제공자는 Google, Microsoft, Amazon Web Services, H2O 등이 있습니다.

---

## AutoML이 하는 일

AutoML은 일반적으로 training data를 입력으로 받아 다음 과정을 자동으로 수행합니다.

```text
Training Data 입력
→ Feature 선택
→ 여러 algorithm 후보 평가
→ Hyperparameter 조합 탐색
→ 최적 성능 모델 선택
→ 최종 모델 학습
→ API 형태로 prediction 제공
```

---

## AutoML의 장점과 한계

| 구분 | 내용 |
| :--- | :--- |
| 장점 | 빠른 모델 개발, 낮은 진입장벽, 반복 실험 자동화 |
| 장점 | feature set, algorithm, hyperparameter 비교 시간을 줄임 |
| 한계 | 비용 발생 가능 |
| 한계 | 세밀한 통제와 해석이 어려울 수 있음 |
| 한계 | 문제 정의, 데이터 품질, label 품질까지 자동으로 해결해주지는 않음 |

AutoML은 모델링 과정을 빠르게 해주지만, 좋은 문제 정의와 좋은 데이터가 없으면 좋은 결과를 만들 수 없습니다. 즉, AutoML은 ML 프로젝트 전체를 대신해주는 것이 아니라, 모델링 단계의 일부를 자동화하는 도구입니다.

---

# 9. 기술 선택을 위한 실무 판단 기준

기술 선택은 다음 흐름으로 판단하는 것이 좋습니다.

```text
1. 사용자 요구사항 확인
   - latency가 중요한가?
   - privacy가 중요한가?
   - cloud/edge 중 어디서 실행해야 하는가?

2. 데이터 요구사항 확인
   - 데이터 규모가 큰가?
   - 실시간 처리가 필요한가?
   - 어떤 pipeline이 필요한가?

3. 모델 유형 확인
   - 전통적 ML인가?
   - deep learning인가?
   - NLP, computer vision 같은 특수 task인가?

4. 팀 역량 확인
   - 팀이 익숙한 언어와 도구는 무엇인가?
   - 새 도구 학습에 시간을 쓸 수 있는가?
   - 채용 가능한 인재 풀이 충분한가?

5. 운영 비용 확인
   - 라이선스 비용은 있는가?
   - training / inference 비용은 어느 정도인가?
   - 장기 유지보수 비용은 감당 가능한가?

6. 배포와 통합 확인
   - API로 제공해야 하는가?
   - 기존 제품과 쉽게 연결되는가?
   - 모니터링과 versioning이 가능한가?
```

---

# 10. 실무적으로 중요한 인사이트

## 1. 기술 선택은 시스템 설계 이후에 해야 한다

Cloud인지 Edge인지, online인지 offline인지, batch prediction인지 online prediction인지가 정해져야 적합한 도구를 고를 수 있습니다. 도구를 먼저 고르고 시스템을 거기에 맞추면 잘못된 선택을 할 가능성이 큽니다.

## 2. 가장 강력한 도구가 항상 최선은 아니다

복잡하고 유연한 도구는 강력하지만 학습 비용과 유지보수 비용이 큽니다. 단순한 문제라면 scikit-learn이나 AutoML처럼 빠르게 구현할 수 있는 도구가 더 좋은 선택일 수 있습니다.

## 3. Python은 기본 선택지에 가깝다

현재 ML과 데이터 과학에서 Python은 가장 널리 쓰이는 언어입니다. 풍부한 library ecosystem, 큰 커뮤니티, 많은 인재 풀 때문에 대부분의 ML 프로젝트에서 가장 현실적인 출발점이 됩니다.

## 4. Jupyter는 실험에 좋지만 production에는 조심해야 한다

Jupyter Notebook은 EDA와 prototype에는 매우 좋습니다. 하지만 테스트, versioning, productionization에는 약점이 있으므로, 최종 서비스 코드는 더 구조화된 script나 package 형태로 옮기는 것이 좋습니다.

## 5. Data tool과 model tool을 분리해서 생각해야 한다

Pandas, NumPy는 데이터 조작과 분석에 강하고, scikit-learn은 전통적 ML 모델링에 적합하며, TensorFlow/PyTorch/Keras는 deep learning에 적합합니다. 하나의 도구가 모든 일을 해결하는 것이 아니라, 각 단계에 맞는 도구 조합이 필요합니다.

## 6. AutoML은 빠른 출발점이지만 만능은 아니다

AutoML은 빠르게 baseline이나 prototype을 만들 때 유용하지만, 데이터 품질, 문제 정의, bias, 운영 설계, 모델 신뢰성 문제를 대신 해결해주지는 않습니다. AutoML을 쓰더라도 ML 프로젝트의 핵심 판단은 여전히 사람이 해야 합니다.

---

# 11. 프로젝트 / 면접에 써먹을 수 있는 문장

ML 기술 선택은 모델링 라이브러리 하나를 고르는 것이 아니라, 시스템 설계 결정과 팀 역량, 데이터 규모, 배포 방식, 비용 구조를 종합해 기술 스택을 결정하는 과정입니다. 일반적으로 Python ecosystem은 ML 프로젝트의 현실적인 출발점이 되며, Jupyter는 실험과 EDA에 적합하지만 production 단계에서는 테스트와 versioning을 고려해 더 구조화된 코드로 전환해야 합니다.

---

# 최종 정리

Module 4 후반부의 핵심은 기술을 유행이나 선호로 고르지 말고, **사용자 요구사항과 ML 시스템 설계 결정에 맞춰 선택해야 한다는 것**입니다. 먼저 cloud/edge, offline/online learning, batch/online prediction 같은 시스템 방향을 정하고, 그 다음 프로그래밍 언어, 데이터 파이프라인 도구, 모델링 라이브러리, API 기술을 선택해야 합니다.

Python은 ML과 데이터 과학의 중심 언어이고, Pandas/NumPy/scikit-learn/matplotlib/NLTK/spaCy는 전통적인 데이터 분석과 ML 작업에 자주 사용됩니다. Deep learning에서는 TensorFlow, PyTorch, Keras가 대표적이며, AutoML은 모델 개발의 반복 작업을 줄여 빠르게 모델을 만들 수 있게 해줍니다. 그러나 어떤 도구를 쓰더라도 좋은 문제 정의, 좋은 데이터, 적절한 시스템 설계가 없으면 좋은 ML 제품을 만들 수 없습니다.