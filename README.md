# 양현고등학교 모의고사 성적 분석 대시보드

매달 모의고사 엑셀 파일을 넣고 스크립트를 실행하면 대시보드가 자동 생성됩니다.

## 폴더 구조

```
yanghyeon-mock/
├── build_dashboard.py    ← 빌드 스크립트
├── template.html         ← 대시보드 HTML 템플릿
├── README.md
├── data/                 ← 여기에 엑셀 파일을 넣으세요
│   ├── 2026.03/
│   │   ├── 1학년.xlsx
│   │   ├── 2학년.xlsx
│   │   └── 3학년.xlsx
│   ├── 2026.05/          ← 5월 시험 추가 시 폴더 생성
│   │   ├── 1학년.xlsx
│   │   ├── 2학년.xlsx
│   │   └── 3학년.xlsx
│   └── 2026.06/          ← 6월, 7월, 9월... 계속 추가
│       └── ...
└── docs/
    └── index.html         ← 자동 생성되는 대시보드
```

## 최초 설정 (한 번만)

### 1. 필수 라이브러리 설치

```bash
pip install pandas openpyxl
```

### 2. GitHub 저장소 생성 및 연결

```bash
# 이 폴더에서 실행
cd yanghyeon-mock
git init
git remote add origin https://github.com/본인계정/yanghyeon-mock.git
git branch -M main
```

### 3. GitHub Pages 설정

- GitHub 저장소 Settings → Pages
- Source: `Deploy from a branch`
- Branch: `main`, 폴더: `/docs`
- Save

## 사용법

### 시험 데이터 추가

```bash
# 1. data/ 아래에 시험 폴더 생성 (yyyy.mm 형식)
mkdir data/2026.05

# 2. 엑셀 파일 복사 (파일명에 "1학년", "2학년", "3학년" 포함)
cp ~/Downloads/5월_1학년_성적.xlsx  data/2026.05/1학년.xlsx
cp ~/Downloads/5월_2학년_성적.xlsx  data/2026.05/2학년.xlsx
cp ~/Downloads/5월_3학년_성적.xlsx  data/2026.05/3학년.xlsx
```

### 대시보드 생성

```bash
# 빌드만 (docs/index.html 생성)
python build_dashboard.py

# 빌드 + GitHub 배포
python build_dashboard.py --push

# 커밋 메시지 지정
python build_dashboard.py --push -m "5월 모의고사 데이터 추가"
```

### 매달 반복 작업 (2분 소요)

```
1. 엑셀 파일을 data/yyyy.mm/ 폴더에 복사
2. python build_dashboard.py --push
3. 끝!
```

## 엑셀 파일 형식

파일명에 "1학년", "2학년", "3학년" 중 하나가 포함되어야 합니다.

- **1학년**: 시트에 "성적 정보"가 포함된 이름. 2행 헤더 + 데이터.
- **2학년**: 시트에 "모의고사" 또는 "2학년"이 포함된 이름. 3행부터 데이터.
- **3학년**: 시트에 "예상점수"가 포함된 이름. 선택과목 정보 포함.

파일 형식이 바뀌면 `build_dashboard.py`의 파서 함수를 수정하세요.

## 문제 해결

| 문제 | 해결 |
|------|------|
| `ModuleNotFoundError: pandas` | `pip install pandas openpyxl` |
| 데이터가 0명으로 나옴 | 엑셀 시트 이름 확인 (파서가 시트를 못 찾는 경우) |
| GitHub Pages 안 열림 | Settings → Pages에서 `/docs` 폴더 설정 확인 |
| 모바일에서 차트 안 보임 | 인터넷 연결 필요 (Chart.js CDN) |
