
# 2. 머신 러닝 프로젝트 관리 (Managing Machine Learning Projects)

Duke University AI Product Management Specialization — **Course 2** 학습 정리 노트.

---

## 폴더 구조

```
2. 머신 러닝 프로젝트 관리/
├── Module1/
│   └── Module1.md
├── Module2/
│   ├── Module2_MLpj.md
│   └── Module2_Teams.md
├── Module3/
│   ├── Module3-data_sourcing.md
│   └── Module3-DataPipeline.md
├── Module4/
│   ├── Module4-1.md
│   └── Module4-2.md
├── Module5/
│   ├── Module5.md
│   └── 넷플릭스의MLOps.md
└── README.md
```

---

## 파일별 내용 요약

### Module 1 — Identifying Opportunities for ML

| 파일 | 내용 |
| :--- | :--- |
| `Module1.md` | ML 도입의 정당성 판단. ML이 가치를 만드는 3가지 방식(Automation, Prediction, Personalization), 휴리스틱 baseline의 중요성, ML vs 휴리스틱 비교 |

### Module 2 — Teams & ML Project Lifecycle

| 파일 | 내용 |
| :--- | :--- |
| `Module2_Teams.md` | ML 프로젝트 팀 구성과 역할 분담 (PM, Data Scientist, Data Engineer, ML Engineer, Domain Expert) |
| `Module2_MLpj.md` | ML 프로젝트의 4단계 라이프사이클 (Problem Definition → Data Sourcing → Modeling → Deployment) |

### Module 3 — Data Sourcing & Pipeline

| 파일 | 내용 |
| :--- | :--- |
| `Module3-data_sourcing.md` | 데이터 출처별 특성과 리스크, 라이선스, 라벨링 전략 (자체/크라우드소싱/약지도) |
| `Module3-DataPipeline.md` | ETL/ELT, Feature Store, 데이터 버저닝, 운영 가능한 데이터 파이프라인 설계 |

### Module 4 — ML System Design

| 파일 | 내용 |
| :--- | :--- |
| `Module4-1.md` | ML 시스템 설계 기본 원칙, 서빙 패턴(배치/온라인/스트리밍) |
| `Module4-2.md` | 비용·지연·정확도 트레이드오프, 확장성 고려사항 |

### Module 5 — Model Lifecycle Management

| 파일 | 내용 |
| :--- | :--- |
| `Module5.md` | 모델 배포 이후의 라이프사이클 관리, 데이터/모델 드리프트, 재학습 트리거, 메타데이터 관리 |
| `넷플릭스의MLOps.md` | Netflix Runway 사례 분석. 모델 메타데이터 통합 관리, 데이터 계보(Lineage) 시각화, 비즈니스 메트릭 기반 모니터링 |

---

## 정리 원칙

모든 노트는 다음 가독성 규칙에 따라 작성되었다.

- **더블 엔터 규칙**: 헤더-본문, 본문-표 사이 빈 줄로 분리
- **표 표준**: 헤더 구분선(`| :--- | :--- |`) 필수, 표 앞뒤 빈 줄
- **수식**: LaTeX 형식(`$...$` 또는 `$$...$$`) 사용

---

## 관련 링크

- 강의: [Duke AI Product Management Specialization](https://www.coursera.org/specializations/ai-product-management-duke)
- 외부 참고: [Netflix Runway — Model Lifecycle Management](https://youtu.be/kvl4lCIMqio)
- 이전 코스: [`1. 제품 관리자를 위한 머신 러닝`](../1.%20제품%20관리자를%20위한%20머신%20러닝)