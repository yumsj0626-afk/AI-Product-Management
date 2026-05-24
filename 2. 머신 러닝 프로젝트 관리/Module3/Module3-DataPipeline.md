
# Managing Machine Learning Projects

## Module 3 — Data Considerations

### Data Pipelines: Governance, Cleaning, Preparation, Reproducibility

---

## 한 줄 요약

ML 프로젝트에서 데이터 문제는 단순히 “데이터를 모으고 정제하는 일”이 아니라, **조직 안에서 데이터를 찾고 접근할 수 있게 만들고, 모델이 신뢰할 수 있는 형태로 정제·가공하며, 나중에도 같은 결과를 재현할 수 있도록 관리하는 전체 체계**입니다. 좋은 데이터 파이프라인은 데이터 접근성, 품질, feature 설계, versioning, lineage까지 함께 포함합니다.

---

# 1. Data Governance & Access

## 핵심 개념

대기업이나 큰 조직에서 데이터 사이언티스트가 자주 겪는 문제는 “데이터가 없어서”가 아니라, **데이터가 여러 부서와 시스템에 흩어져 있어 찾기 어렵고 접근하기 어렵다는 것**입니다.

많은 조직에서는 각 부서가 자체 시스템을 구매하고 운영하면서 데이터를 따로 저장합니다. 이 데이터들은 서로 다른 위치, 다른 schema, 다른 관리 방식으로 존재하기 때문에 외부 팀이 접근하기 어렵고, 설령 접근하더라도 데이터의 의미를 이해하기 어렵습니다. 이런 상태를 **data silo**라고 부릅니다.

```text
부서별 시스템
→ 각자 다른 데이터 저장
→ 서로 다른 schema와 관리 방식
→ 외부 팀이 접근하거나 이해하기 어려움
→ data silo 발생
```

ML을 막 시작하는 회사라면 무작정 데이터 사이언티스트를 많이 뽑기보다, 먼저 데이터 silo를 줄이고 데이터가 실제로 사용 가능한 상태인지 점검하는 것이 중요합니다. 데이터가 접근 가능하고 정리되어 있지 않으면, 데이터 사이언스 팀은 모델링보다 데이터 접근과 정리에 대부분의 시간을 쓰게 됩니다.

---

## Data Silo를 줄이는 3가지 요소

Data silo를 해결하는 것은 단순히 데이터 웨어하우스를 도입하는 기술 문제가 아닙니다. 원본 강의에서는 세 가지 축을 강조합니다.

| 요소 | 설명 |
| :--- | :--- |
| Cultural Change | 조직 전체가 데이터를 공유 자산으로 보는 문화가 필요함 |
| Technology | 데이터를 중앙화하고 쉽게 쿼리할 수 있는 기술 기반이 필요함 |
| Data Stewardship & Access | 데이터 관리 책임자, 데이터 카탈로그, 접근 절차가 필요함 |

첫째, 문화적 변화가 필요합니다. 각 부서가 “내 데이터”라고 막아두는 문화에서는 데이터 활용이 어렵습니다. 이때 executive sponsor나 고위 책임자가 데이터 접근성 개선의 champion 역할을 해야 하고, 부서들이 데이터를 공유하도록 교육과 인센티브를 제공해야 합니다.

둘째, 기술적 기반이 필요합니다. 데이터 웨어하우스나 중앙화된 데이터 저장소를 통해 데이터를 한곳에서 관리하고, 사용자가 필요한 데이터를 쉽게 query할 수 있어야 합니다.

셋째, data stewardship이 필요합니다. 누가 데이터를 관리하고, 품질을 유지하고, 데이터가 어디에 있는지 알려주는지 명확해야 합니다. 데이터 catalog나 data map은 사용자가 “우리 조직에 어떤 데이터가 있고, 어디에 있으며, 어떻게 접근할 수 있는지” 이해하게 해주는 핵심 도구입니다.

---

## Facebook 사례: Hive와 Self-Service Data Access

Facebook은 2007~2010년 사이 사용자와 데이터가 폭발적으로 증가하면서, 직원들의 데이터 query 요청도 급증했습니다. 2010년에는 하루 평균 10,000건의 데이터 query 요청이 발생했습니다.

초기에는 Hadoop cluster로 저장 문제를 해결했지만, Hadoop에 접근하려면 MapReduce 프로그램을 작성할 수 있어야 했습니다. 이는 데이터 엔지니어가 아닌 직원들에게 큰 진입장벽이었습니다.

이를 해결하기 위해 Facebook의 데이터 엔지니어링 팀은 Hadoop 위에서 SQL처럼 query할 수 있는 **Hive**를 개발했습니다. Hive는 직원들이 MapReduce를 직접 작성하지 않아도 데이터를 조회할 수 있게 만들었고, Facebook은 self-service data access 모델로 전환했습니다.

```text
데이터 증가
→ Hadoop으로 저장 문제 해결
→ 하지만 MapReduce가 접근 장벽이 됨
→ Hive 개발
→ SQL 기반 query 가능
→ self-service access
→ 데이터 엔지니어링 팀이 병목에서 벗어남
```

이 사례의 핵심은 데이터 접근성 개선이 단순한 도구 도입이 아니라, 조직의 일하는 방식 자체를 바꾼다는 점입니다. Facebook은 교육과 hackathon을 통해 직원들이 데이터를 직접 활용하도록 유도했고, 데이터 엔지니어링 팀이 모든 요청을 처리하는 병목 구조를 줄였습니다.

---

# 2. Spotify Lexikon 사례: Data Discovery를 제품처럼 개선하기

## 핵심 개념

Spotify의 Lexikon 사례는 데이터 접근성의 핵심이 단순히 “데이터 목록을 제공하는 것”이 아니라, **데이터 사용자가 실제로 필요한 데이터를 발견하고, 이해하고, 바로 사용할 수 있게 만드는 경험 설계**라는 점을 보여줍니다.

Spotify는 GCP와 BigQuery로 이동하면서 dataset 생성이 폭발적으로 늘었고, 데이터 사이언티스트와 analyst도 증가했습니다. 하지만 dataset의 소유자나 문서화가 명확하지 않아 데이터 과학자들이 필요한 데이터를 찾는 데 많은 시간을 쓰고 있었습니다. 이를 해결하기 위해 Spotify는 데이터와 insight를 찾을 수 있는 내부 라이브러리인 **Lexikon**을 만들었습니다.

처음에는 dataset과 research/analysis 결과를 검색하고 탐색할 수 있는 중앙 catalog를 제공했습니다. 그러나 catalog를 만들었음에도 데이터 사이언티스트들은 여전히 data discovery를 큰 pain point로 보고했습니다. 즉, “검색 가능한 목록”만으로는 충분하지 않았던 것입니다.

---

## Spotify가 발견한 핵심 문제

Spotify 팀은 data discovery를 개선하기 위해 세 가지 방향으로 Lexikon을 개선했습니다.

| 개선 방향 | 핵심 의미 |
| :--- | :--- |
| User Intent 이해 | 사용자가 막연히 탐색하는지, 정확한 대상을 찾는지 구분 |
| People을 통한 Knowledge Exchange | 문서화된 정보만으로 부족한 지식을 사람 연결로 보완 |
| Dataset 사용 시작 지원 | 데이터를 찾은 뒤 실제로 어떻게 써야 하는지 돕기 |

---

## Low-intent vs High-intent Data Discovery

Spotify는 data discovery에도 사용자의 intent 수준이 다르다는 점을 발견했습니다.

**Low-intent discovery**는 사용자가 정확히 무엇을 찾는지 모르는 상태입니다. 예를 들어 새로 입사했거나 새로운 프로젝트를 시작한 데이터 사이언티스트는 “우리 팀에서 많이 쓰는 dataset이 뭐지?”, “회사에서 널리 쓰이는 dataset은 뭐지?”, “내가 알아야 하는 dataset은 뭐지?” 같은 넓은 질문을 가집니다.

이를 위해 Lexikon은 homepage에서 개인화된 dataset recommendation을 제공했습니다. 추천에는 널리 쓰이는 dataset, 최근 사용한 dataset, 소속 팀에서 많이 쓰는 dataset, 아직 쓰지 않았지만 유용할 수 있는 dataset 등이 포함되었습니다. 흥미로운 점은 복잡한 NLP나 topic modeling보다, 실제 사용 통계 기반의 단순한 heuristic recommendation이 사용자 피드백상 잘 작동했다는 점입니다.

**High-intent discovery**는 사용자가 비교적 정확히 무엇을 찾는지 아는 상태입니다. 예를 들어 dataset 이름, 특정 schema field, 특정 topic, BigQuery project, 특정 동료가 사용했던 dataset 등을 찾는 경우입니다.

이를 위해 Lexikon은 search ranking을 개선했습니다. Spotify는 수만 개 dataset이 있어도 실제 사용은 상위 일부 dataset에 집중된다는 점을 발견했고, 검색 결과에 popularity를 더 강하게 반영했습니다. 그 결과 사용자는 검색 결과를 더 관련성 높게 느꼈고, “다른 사람들이 많이 쓰는 dataset”이라는 사실 때문에 더 신뢰할 수 있었습니다.

---

## Data Discovery에서 사람의 역할

Lexikon의 첫 버전은 knowledge를 최대한 문서화하고 catalog화하는 방식이었습니다. 하지만 Spotify는 data discovery에서 사람 간 지식 교환이 여전히 중요하다는 점을 발견했습니다.

문서가 잘 되어 있어도, 사용자는 종종 “이 데이터에 대해 잘 아는 사람이 누구지?”를 알고 싶어 합니다. 특히 신입 직원이나 새로운 팀으로 이동한 사람은 특정 주제의 전문가를 찾기 어렵습니다.

그래서 Lexikon은 특정 keyword와 관련된 expert를 찾을 수 있는 기능을 추가했습니다. 이때 expert는 단순히 직함으로 정해지는 것이 아니라, dataset 소유, dashboard 소유, research report 작성, A/B test 실행, query/view 활동 등 insight production과 consumption 활동을 바탕으로 추정되었습니다. 특히 단순 조회보다 dashboard 소유나 report 작성처럼 생산 활동에 더 높은 가중치를 두었습니다.

또한 Spotify는 사람들이 Slack에서 dataset에 대해 이야기한다는 점을 받아들였습니다. 이를 막기보다 Lexikon Slack Bot을 만들어, Slack에 Lexikon dataset 링크가 공유되면 dataset 이름, owner, description, usage stats, data lifecycle, access tier, 주요 schema field, access 요청 링크 등을 자동으로 보여주게 했습니다.

이 사례의 핵심은 데이터 discovery를 catalog 안에만 가두지 않고, 실제 협업이 일어나는 Slack 같은 도구와 연결했다는 점입니다.

---

## Dataset을 찾은 뒤 실제 사용을 돕기

Spotify는 사용자가 dataset을 찾은 뒤에도 “이 데이터를 어떻게 써야 하지?”에서 막힌다는 것을 발견했습니다. 그래서 dataset page에서 사용 시작을 돕는 기능을 강화했습니다.

| 기능 | 역할 |
| :--- | :--- |
| Schema-field consumption statistics | 어떤 field가 많이 query되는지 보여줌 |
| Recent Queries | 해당 dataset이 실제로 어떻게 사용되는지 최신 query로 보여줌 |
| Tables commonly joined | 이 dataset과 자주 join되는 table을 보여줌 |

Schema field가 수십~수백 개 있는 dataset에서는 어떤 field가 중요한지 파악하기 어렵습니다. field별 query 수와 unique user 수를 보여주면, 사용자는 많이 쓰이는 field부터 확인할 수 있습니다.

또한 curated example query는 관리가 어렵고 금방 오래된 정보가 될 수 있습니다. 그래서 Lexikon은 사람이 작성해둔 예시 query 대신, 실제 최근 query를 검색할 수 있게 했습니다. 이는 데이터가 실제로 어떻게 쓰이는지 보여주는 더 현실적인 방식입니다.

마지막으로 하나의 dataset만으로는 분석이 끝나지 않는 경우가 많기 때문에, 자주 join되는 table을 보여주는 기능도 추가했습니다.

---

## Spotify 사례의 핵심 인사이트

Spotify Lexikon 사례에서 가져갈 핵심은 다음입니다.

```text
좋은 data catalog는 단순 목록이 아니다.
→ 사용자의 탐색 의도에 맞게 discovery 경로를 제공해야 한다.

문서화만으로는 부족하다.
→ 실제로 데이터를 아는 사람과 연결될 수 있어야 한다.

데이터를 찾는 것만으로는 부족하다.
→ 발견한 dataset을 바로 이해하고 사용할 수 있게 해야 한다.

복잡한 ML 추천보다 단순한 사용 통계 기반 heuristic이 더 잘 먹힐 때도 있다.
→ 문제에 맞는 단순한 방법부터 검증해야 한다.
```

---

# 3. Data Cleaning

## 핵심 개념

ML 프로젝트에서 가장 흔한 문제 중 하나는 messy data입니다. 데이터가 지저분하면 좋은 모델을 만들기 어렵습니다. 원본 강의에서는 messy data의 대표적인 문제로 missing data, anomalies/outliers, 잘못된 data mapping을 다룹니다.

```text
Messy Data
→ missing data
→ anomalies / outliers
→ 잘못된 source mapping
→ 모델 품질 저하
```

특히 여러 소스의 데이터를 합치고 집계할 때 mapping이 잘못되면, 모델이 잘못된 관계를 학습할 수 있습니다.

---

## Missing Data

데이터가 누락되는 이유는 다양합니다. 사용자가 웹 form의 일부 항목을 입력하지 않았을 수도 있고, 사람이 수기로 입력하는 과정에서 누락이 생겼을 수도 있습니다. 센서 데이터라면 전원 문제, 통신 오류, 장비 고장 때문에 일부 값이 사라질 수 있습니다.

중요한 점은 missing data가 모두 같은 의미가 아니라는 것입니다. missing data가 어떤 방식으로 발생했는지에 따라 bias 위험이 달라집니다.

| 유형 | 의미 | 위험 |
| :--- | :--- | :--- |
| Missing Completely at Random | 누락이 다른 변수나 값과 관련 없이 무작위로 발생 | bias 위험이 상대적으로 낮음 |
| Missing at Random | 누락 가능성이 다른 feature와 관련됨 | bias 위험이 있음 |
| Missing Not at Random | 누락 가능성이 해당 feature의 실제 값과 직접 관련됨 | bias 위험이 큼 |

예를 들어 센서 전원 오류로 랜덤하게 데이터가 누락된 경우는 Missing Completely at Random에 가깝습니다. 반면 의료 설문에서 남성이 우울증 관련 질문에 덜 답하는 경우는 누락 여부가 성별이라는 다른 feature와 관련되어 있으므로 Missing at Random입니다. 상품 리뷰에서 불만족한 사용자만 적극적으로 평점을 남긴다면, missing 여부가 실제 만족도 값과 직접 관련되므로 Missing Not at Random입니다.

Missing at Random과 Missing Not at Random은 dataset과 모델에 bias를 만들 수 있으므로 특히 조심해야 합니다.

---

## Missing Data 처리 방법

Missing data를 처리하는 방법은 상황에 따라 다릅니다.

| 방법 | 설명 | 주의점 |
| :--- | :--- | :--- |
| Remove | 결측치가 있는 row나 column 제거 | 데이터가 충분할 때만 안전 |
| Flag | 결측 여부 자체를 feature로 표시 | 결측이 의미 있는 신호일 때 유용 |
| Replace | 평균, 중앙값 등으로 대체 | 단순하지만 분포를 왜곡할 수 있음 |
| Forward / Backward Fill | 이전 값 또는 다음 값으로 채움 | 시계열·센서 데이터에서 자주 사용 |
| Infer | 다른 feature로 missing value를 예측 | 더 정교하지만 추가 모델링 필요 |

중요한 것은 결측치를 무조건 지우는 것이 아니라, 왜 누락되었고 누락 자체가 의미를 가지는지 먼저 판단하는 것입니다. 어떤 경우에는 “값이 없다”는 사실 자체가 모델에 중요한 signal이 될 수 있습니다.

---

## Outliers

Outlier는 다른 데이터 포인트와 멀리 떨어져 있는 값입니다. feature 값에서 나타날 수도 있고 target 값에서 나타날 수도 있습니다. 문제는 outlier가 모델에 지나치게 큰 영향을 주어 모델의 형태를 왜곡할 수 있다는 점입니다.

하지만 outlier를 무조건 제거하면 안 됩니다. 어떤 outlier는 오류가 아니라 실제로 발생한 극단적 사건일 수 있습니다. 예를 들어 재난, 폭풍, 장비 고장, 금융 위기처럼 드물지만 중요한 사건을 예측해야 하는 문제라면 outlier를 제거하는 것이 오히려 모델의 학습 능력을 떨어뜨릴 수 있습니다.

또한 outlier는 항상 “값이 극단적으로 큰 것”만 의미하지 않습니다. 전체 범위 안에 있는 값이라도, 시간적 패턴이나 맥락상 이상하면 outlier일 수 있습니다. 예를 들어 온도가 전체적으로 가능한 범위 안에 있더라도, 특정 시점의 정상적인 흐름에서 갑자기 크게 떨어지면 contextual outlier로 볼 수 있습니다.

---

## Outlier 처리 원칙

Outlier를 발견하면 먼저 root cause를 파악해야 합니다.

```text
1. 이 값은 실제로 발생한 사건인가?
2. 아니면 측정 오류, 입력 오류, mapping 오류인가?
3. 실제 사건이라면 모델이 배워야 하는 중요한 패턴인가?
4. 오류라면 제거할 것인가, 더 그럴듯한 값으로 조정할 것인가?
```

Outlier 탐지에는 통계적 test나 visualization을 사용할 수 있습니다. scatter plot, box plot 같은 시각화는 outlier를 빠르게 발견하는 데 유용합니다. 다만 발견 이후의 처리는 자동화하기보다 도메인 맥락을 함께 봐야 합니다.

---

# 4. Preparing Data for Modeling

## 핵심 개념

Raw data는 보통 그대로 ML 모델 학습에 사용할 수 없습니다. 모델링 전에 데이터 정제, EDA, feature engineering, feature selection, scaling, categorical encoding 같은 준비 과정이 필요합니다.

```text
Raw Data
→ Cleaning
→ EDA
→ Feature Engineering
→ Feature Selection
→ Scaling / Encoding
→ Modeling-ready Data
```

이 과정은 단순한 전처리 작업이 아니라, 모델 성능을 결정하는 핵심 단계입니다.

---

## Exploratory Data Analysis, EDA

EDA의 목적은 데이터의 문제와 패턴을 이해하는 것입니다. EDA에서는 통계적 방법과 시각화 방법을 모두 사용할 수 있습니다.

EDA에서 확인해야 할 것은 크게 세 가지입니다.

| 확인 대상 | 설명 |
| :--- | :--- |
| Feature 분포 | 각 입력 변수가 어떤 범위와 분포를 가지는지 확인 |
| Target 분포 | 예측하려는 값이나 label이 어떻게 분포하는지 확인 |
| Feature 간 / Feature-Target 관계 | 어떤 feature가 target과 관련이 있는지, feature끼리 어떤 관계가 있는지 확인 |

Scatter plot, correlation matrix 같은 시각화는 feature 간 관계나 feature와 target 간 관계를 파악하는 데 유용합니다. EDA는 나중에 어떤 feature를 만들고 선택할지 결정하는 기초가 됩니다.

---

## Feature Engineering

Feature engineering은 모델 학습에 사용할 feature를 만들고 표현하는 과정입니다. 원본 강의에서 가장 중요한 메시지는 이것입니다.

> 좋은 feature set이 있으면 단순한 모델로도 꽤 괜찮은 결과를 얻을 수 있지만, 잘못된 feature set을 쓰면 아무리 복잡한 모델을 사용해도 좋은 결과를 얻기 어렵다.

Feature는 원래 데이터에 자연스럽게 존재하는 속성일 수도 있고, 새롭게 만들어야 하는 값일 수도 있습니다. 예를 들어 온도, 습도, 길이, 폭, 제조연도 같은 값은 자연스러운 feature입니다. 반면 텍스트 데이터를 모델에 넣기 위해 숫자 벡터로 바꾸는 과정은 feature representation을 설계하는 일입니다.

---

## Feature Engineering 예시

### 1. 건물 에너지 소비 예측

사무실 건물의 일일 에너지 소비량을 날씨 조건으로 예측한다고 하면, 어떤 weather parameter를 사용할지 결정해야 합니다. 온도, 습도, 풍속, 구름량 등이 후보가 될 수 있습니다.

여기서 단순히 feature를 고르는 것뿐 아니라, feature를 어떻게 표현할지도 중요합니다. 예를 들어 구름량을 0/1로 표현할지, 1~5 scale로 표현할지 선택해야 합니다. 날씨 정보를 일평균으로 쓸지, 업무 시간 평균으로 쓸지, 시간 단위 값으로 쓸지도 결정해야 합니다.

또한 feature 간 interaction도 고려해야 합니다. 여름에 햇빛이 강하면 냉방 부하가 증가해 에너지 소비가 늘 수 있지만, 겨울에 햇빛이 강하면 난방 부하가 줄어 에너지 소비가 감소할 수 있습니다. 즉, 같은 feature라도 season과 결합될 때 의미가 달라질 수 있습니다.

### 2. 자전거 공유 수요 예측

자전거 공유 네트워크의 시간별 수요를 예측한다면 날씨뿐 아니라 시간 정보도 중요합니다. hour of day, day of week, business day/weekend, holiday, month, season 같은 feature를 고려할 수 있습니다.

이 사례의 핵심은 “시간”이라는 정보를 단순히 하나의 값으로 넣는 것이 아니라, 수요 패턴과 관련 있는 방식으로 여러 feature로 표현해야 한다는 점입니다.

---

## Feature Selection

Feature selection은 만든 feature 후보들 중 실제 모델에 사용할 feature subset을 고르는 과정입니다. feature 수를 줄이면 모델 복잡도를 줄이고, overfitting 위험을 낮추며, training time을 줄이고, 모델 해석 가능성도 높일 수 있습니다.

하지만 중요한 feature를 빠뜨리면 모델 성능은 크게 제한됩니다. 따라서 feature selection은 “적게 쓰는 것”이 목표가 아니라, **필요 없는 feature를 줄이되 중요한 signal은 놓치지 않는 것**이 목표입니다.

| 방법 | 설명 | 특징 |
| :--- | :--- | :--- |
| Filter Methods | 데이터의 통계적 특성으로 feature 선택 | 빠르고 저렴하며 초기에 명백히 불필요한 feature 제거에 유용 |
| Wrapper Methods | feature subset별로 여러 모델을 학습해 성능 비교 | 직접적이지만 계산 비용이 큼 |
| Embedded Methods | 모델 학습 과정에서 feature 중요도를 파악 | decision tree, random forest의 split 기여도 등이 예시 |

마지막으로 모델에 넣기 위해 값의 scale을 맞추고, 문자열 categorical variable을 숫자 코드로 변환해야 할 수 있습니다. 예를 들어 season이 “spring”, “summer” 같은 문자열이면 모델이 처리할 수 있도록 encoding이 필요합니다.

---

# 5. Reproducibility & Versioning

## 핵심 개념

ML 프로젝트에서 reproducibility는 나중에 자신이나 다른 사람이 같은 결과를 다시 만들어낼 수 있는 능력입니다. 이는 ML 프로젝트에서 특히 중요합니다. ML 프로젝트는 복잡한 data pipeline, 여러 단계의 transformation, 다양한 model training 설정을 포함하고, 학습 시점이나 방식에 따라 결과가 달라질 수 있기 때문입니다.

재현성이 없으면 문제가 발생했을 때 원인을 찾기 어렵고, 팀원이 바뀌거나 모델이 다른 팀으로 넘어갔을 때 유지보수가 어려워집니다. 학술 환경에서는 재현성이 모델 결과의 신뢰도를 높이는 핵심 조건이기도 합니다.

---

## Reproducibility를 위해 필요한 것

원본 강의는 reproducibility를 위해 세 가지를 강조합니다.

| 요소 | 설명 |
| :--- | :--- |
| Documentation | 모델, pipeline, dependency, 의사결정 기록 |
| Data Lineage | raw data부터 최종 feature까지 데이터 흐름 추적 |
| Versioning | code, data, pipeline, model의 버전 관리 |

Documentation에는 모델과 pipeline의 기능뿐 아니라, 코드 dependency, 데이터와 모델 사이의 dependency도 포함되어야 합니다.

---

## Data Lineage

Data lineage는 데이터가 어디서 왔고, 어떤 과정을 거쳐, 어떤 형태로 모델에 사용되었는지 추적하는 것입니다.

```text
Raw Data Source
→ ETL
→ Data Warehouse
→ Data Pipeline
→ Feature Engineering / Selection
→ Train/Test Split
→ Model Training
```

Data lineage가 중요한 이유는 다음과 같습니다.

| 이유 | 설명 |
| :--- | :--- |
| Debugging | 모델 성능 문제가 생겼을 때 데이터 원인을 추적할 수 있음 |
| Migration | 시스템 이전이나 데이터 이동 시 영향을 파악하기 쉬움 |
| Trust | 사용자가 데이터의 출처와 변환 과정을 알면 신뢰하기 쉬움 |
| Compliance | 금융 등 일부 산업에서는 데이터 추적이 규제상 필요함 |

Data lineage map은 보통 높은 수준의 전체 흐름 지도와, 각 단계별 세부 지도로 구성됩니다. 여기에는 데이터 source, 수집 기간, 수집 방식, 데이터 특성, table 간 관계, feature와 table의 관계, transformation, processing, 데이터 저장 위치 등이 기록됩니다.

---

## Versioning

일반 소프트웨어 프로젝트에서는 code versioning이 중요합니다. 하지만 ML 프로젝트에서는 code뿐 아니라 data, data pipeline, model도 함께 versioning해야 합니다.

ML 모델을 만들 때는 수많은 작은 실험을 반복합니다. feature set, algorithm, hyperparameter, training data가 계속 바뀌기 때문에 무엇을 시도했고, 무엇이 잘 되었고, 무엇이 실패했는지 추적하지 않으면 같은 실험을 반복하거나 문제 발생 시 되돌릴 수 없습니다.

```text
Code Version
+ Data Version
+ Pipeline Version
+ Model Version
= Reproducible ML Project
```

Model versioning은 특히 production 운영에서 중요합니다. 배포한 모델의 성능이 떨어지거나 문제가 생기면 이전 버전으로 rollback할 수 있어야 합니다. 또한 여러 모델을 동시에 비교하는 champion-challenger test를 운영할 수도 있습니다. 이 방식에서는 현재 production model인 champion과 새로운 challenger model을 병렬로 비교하고, 성능이 더 좋은 모델로 교체합니다.

---

# 6. 전체 흐름으로 보는 Data Pipeline

Module 3 후반부를 하나의 흐름으로 보면 다음과 같습니다.

```text
조직 내 데이터 발견과 접근
→ data silo 해소
→ data catalog / data map / self-service access
→ raw data 수집
→ data cleaning
→ EDA
→ feature engineering
→ feature selection
→ scaling / encoding
→ data lineage 기록
→ data / code / model versioning
→ reproducible modeling
```

이 흐름에서 중요한 것은 각 단계가 따로 노는 것이 아니라는 점입니다. 데이터 접근이 어렵다면 모델링 이전에 병목이 생기고, cleaning이 부족하면 모델 품질이 떨어지며, lineage와 versioning이 없으면 나중에 결과를 재현하거나 문제를 디버깅할 수 없습니다.

---

# 7. 실무적으로 중요한 인사이트

## 1. 데이터 접근성은 ML 조직의 생산성을 결정한다

데이터가 조직 안에 존재하더라도 찾기 어렵고 접근하기 어렵다면, 데이터 사이언티스트는 모델링보다 데이터 탐색과 접근 요청에 시간을 쓰게 됩니다. ML을 잘하는 조직은 데이터 자체뿐 아니라 데이터 discovery와 access 경험을 제품처럼 관리합니다.

## 2. 데이터 catalog는 목록이 아니라 사용 경험이다

Spotify Lexikon 사례처럼 좋은 data catalog는 dataset 이름과 설명만 제공하는 것이 아닙니다. 사용자의 intent를 이해하고, 많이 쓰이는 dataset을 추천하고, expert를 연결하고, 실제 query와 자주 join되는 table까지 보여줘야 사용자가 dataset을 실제 분석에 활용할 수 있습니다.

## 3. Missing data는 단순한 빈칸이 아니라 bias의 신호일 수 있다

결측치가 완전히 무작위인지, 다른 feature와 관련되어 있는지, 값 자체와 관련되어 있는지에 따라 모델에 미치는 영향이 달라집니다. 특히 Missing at Random과 Missing Not at Random은 데이터와 모델에 bias를 만들 수 있습니다.

## 4. Outlier는 무조건 제거하면 안 된다

Outlier는 오류일 수도 있지만, 실제로 중요한 극단적 사건일 수도 있습니다. 특히 극단 상황을 예측해야 하는 모델에서는 outlier를 제거하면 오히려 모델이 중요한 패턴을 배우지 못할 수 있습니다.

## 5. 좋은 feature는 복잡한 모델보다 중요할 수 있다

좋은 feature set이 있으면 단순한 모델도 쓸 만한 성능을 낼 수 있습니다. 반대로 중요한 feature가 빠져 있으면 복잡한 모델을 써도 성능 한계가 뚜렷합니다.

## 6. Feature selection은 단순히 feature를 줄이는 작업이 아니다

Feature selection의 목적은 복잡도와 overfitting을 줄이면서도 중요한 signal을 보존하는 것입니다. 불필요한 feature는 제거해야 하지만, 핵심 feature를 빼먹으면 모델 성능은 구조적으로 제한됩니다.

## 7. 재현성은 나중을 위한 보험이다

모델이 production에서 문제가 생겼을 때, 어떤 데이터와 어떤 pipeline, 어떤 feature, 어떤 model version으로 학습했는지 모르면 디버깅이 거의 불가능합니다. ML 프로젝트에서는 data lineage와 versioning이 선택 사항이 아니라 운영 가능성을 위한 필수 조건입니다.

---

# 8. 프로젝트 / 면접에 써먹을 수 있는 문장

ML 프로젝트에서 데이터 파이프라인은 단순한 전처리 코드가 아니라, 데이터 접근성, 품질 관리, feature engineering, data lineage, versioning을 포함하는 운영 체계입니다. 좋은 ML 조직은 데이터 silo를 줄이고 self-service access와 data catalog를 제공하며, 모델링 단계에서는 missing data와 outlier의 원인을 분석하고, feature set과 데이터 변환 과정을 versioning하여 결과를 재현 가능하게 관리합니다.

---

# 최종 정리

Module 3의 후반부는 ML 프로젝트에서 데이터가 “모델에 넣기 전의 준비물”이 아니라, 프로젝트 성공을 좌우하는 핵심 인프라라는 점을 보여줍니다. 데이터가 어디에 있는지 찾을 수 없고, 접근 권한이 복잡하고, 품질 문제가 정리되지 않았고, 어떤 변환을 거쳤는지 추적되지 않는다면 좋은 모델을 만들기도 어렵고 운영하기도 어렵습니다.

결국 좋은 데이터 파이프라인은 데이터를 모으는 것에서 끝나지 않습니다. 조직이 데이터를 발견하고 접근할 수 있게 만들고, messy data를 bias와 품질 관점에서 정리하며, feature를 신중하게 설계하고, 데이터와 모델의 버전을 추적해 나중에도 같은 결과를 재현할 수 있게 만드는 전체 시스템입니다.