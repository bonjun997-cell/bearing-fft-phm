# 베어링 진동 결함 진단 — FFT & 엔벨로프 분석

CWRU(Case Western Reserve University) 공개 베어링 데이터셋을 활용해  
**raw FFT → 엔벨로프 분석** 순서로 결함 진단 신뢰도를 개선한 과정을 담은 포트폴리오입니다.

## 핵심 결과

| 방법 | BPFO 결함/정상 배율 | 판별 가능 여부 |
|---|---|---|
| raw FFT | 0.4× (정상이 오히려 더 강함) | ❌ 불가 |
| **엔벨로프 분석** | **4,165×** | ✅ 명확 |

## 분석 흐름

```
CWRU 베어링 진동 데이터 (.mat)
│
├── 1. 시간 영역 비교
│      정상: 고른 진동 / 결함: 규칙적 충격 스파이크 확인
│
├── 2. raw FFT
│      BPFO(107.4 Hz) 위치 봉우리 확인 시도
│      → 축 고조파·노이즈가 섞여 판별 불명확
│
└── 3. 엔벨로프 분석 (Hilbert Transform)
       ① 대역통과 필터 (2000~5000 Hz 공진 대역)
       ② Hilbert transform → 포락선(충격 크기) 추출
       ③ 포락선 FFT → BPFO 1×·2×·3×·4× 고조파 선명 검출
```

## 사용 데이터

- **출처**: [CWRU Bearing Data Center](https://engineering.case.edu/bearingdatacenter)
- **베어링**: 6205-2RS JEM SKF (드라이브엔드)
- **샘플링**: 12,000 Hz / 회전속도: 1,797 RPM
- **파일**:
  - `97.mat` — 정상 베어링 (0 HP 부하)
  - `130.mat` — 외륜결함 0.021인치 (0 HP 부하)

## 결함 주파수 (BPFO)

```
BPFO = (n/2) × RPM/60 × (1 - Bd/Pd × cosθ)
     = (9/2) × 29.95 × (1 - 0.3126/1.537)
     ≈ 107.4 Hz
```

## 파일 구성

```
bearing-fft-phm/
└── bearing_fft_cwru.ipynb   # 분석 노트북 (Colab에서 바로 실행 가능)
```

## 실행 방법

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/bonjun997-cell/bearing-fft-phm/blob/main/bearing_fft_cwru.ipynb)

1. 위 버튼 클릭 또는 Colab에서 파일 업로드
2. **모두 실행** (셀 순서대로)
3. 데이터 자동 다운로드 후 그래프 출력

## 배운 점

- raw FFT만으로는 초기 베어링 결함 검출이 어려움 (노이즈 오염)
- 엔벨로프 분석이 ISO 18436-2 현장 표준인 이유를 수치로 직접 확인
- 결함 크기(0.007" vs 0.021")에 따라 검출 신뢰도가 크게 달라짐 → 조기 모니터링의 필요성

## 관련 역량

- ISO 18436-2 진동 상태감시·진단 교육 이수 (신호이앤티, 2023)
- 산업용 펌프·회전기계 설계·생산기술 실무 (신신기계, 2022~2024)
- Python 신호처리 (numpy, scipy, matplotlib)
