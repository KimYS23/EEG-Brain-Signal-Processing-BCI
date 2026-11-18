# EEG-Brain-Signal-Processing-BCI
Clinical EEG Preprocessing Pipeline (MNE, ICA, ICLabel) &amp; SSVEP Frequency Classification (CCA)
# EEG Brain Signal Processing & BCI Classification

임상 뇌파(EEG) 생체신호의 **노이즈 제거 및 자동화 전처리 파이프라인 구축**부터 **뇌-컴퓨터 인터페이스(BCI) 주파수 분류 알고리즘 구현**까지 다룬 프로젝트 아카이브입니다.

---

### 1. 임상 뇌파(EEG) 자동화 전처리 파이프라인 구축 (`0420_practice.ipynb`)
* **개요**: OpenNeuro 파킨슨병 환자 및 정상 대조군 뇌파 데이터셋(`ds002778`, BIDS 표준 형식)을 활용한 생체신호 정제 및 아티팩트 제거 파이프라인 구현
* **주요 수행 내용**:
  * **표준 데이터 로드 및 채널 설정**: `MNE-Python` 및 `mne-bids`를 활용해 64채널 임상 뇌파 데이터를 로드하고 표준 전극 몽타주(`standard_1020`) 매핑 수행
  * **다단계 주파수 필터링**: High-pass(`0.5Hz`), Low-pass(`40Hz`) 및 전원 노이즈 제거를 위한 Notch Filter(`50Hz`, `100Hz`)를 적용하고 전후 PSD(전력 스펙트럼 밀도) 변화 검증
  * **AI 기반 아티팩트 자동 제거**: 2단계 `ICA(독립성분분석)`와 `MNE-ICLabel`을 결합하여 안구 움직임(EOG), 심전도(ECG), 근전도(Muscle) 노이즈 성분을 자동 분류·제거하고 `Autoreject`로 불량 구간 보간 처리

---

### 2. SSVEP 기반 시각 자극 주파수 분류 알고리즘 구현 (`SSVEP_CCA_prac.ipynb`)
* **개요**: Tsinghua SSVEP Benchmark 뇌파 데이터셋(64채널, 40개 시각 자극 주파수, 240 Trials)을 활용한 신호 처리 및 주파수 판별 모델 구현
* **주요 수행 내용**:
  * **생체신호 필터링 및 스펙트럼 분석**: 5차 Butterworth Bandpass Filter(`5~45Hz`)를 설계하여 신호를 정제하고, `FFT(고속푸리에변환)` 시각화를 통해 시각피질(`Oz` 채널)의 자극 주파수 및 고조파(Harmonics) 특성 확인
  * **정준상관분석(CCA) 알고리즘 직접 구현**: 다중 고조파 사인·코사인 참조 신호(Reference Signals)를 생성하고, 고유값 분해(`scipy.linalg.eigh`) 기반의 `myCCA` 함수를 직접 구현하여 다채널 뇌파와 참조 주파수 간 최대 상관계수 산출
  * **채널 선택(Channel Selection)을 통한 성능 고도화**: 후두엽 시각피질 핵심 채널(`O1`, `Oz`, `O2`) 집중 추출 및 필터링 적용으로 40개 클래스 다중 분류 정확도를 **6.67%(원본 전체 채널) → 69.17%(전처리 및 핵심 채널 적용)**로 약 10배 향상
