# 포트폴리오 관리 백엔드 API

사용자의 잔고와 위험 성향을 기반으로 포트폴리오 자문 기능을 제공하는 백엔드 API 프로젝트입니다.  
회원가입, 입출금, 증권 관리, 자문 요청 등의 기능을 구현하여 금융 시스템의 흐름을 이해하고 백엔드 역량을 검증할 수 있습니다.

## 프로젝트 개요

- 회원 시스템 (회원가입, 로그인, 로그아웃)
- 입출금 기능 및 거래 내역 기록
- 위험도 기반 자문 요청 기능
- 포트폴리오 구성 및 증권 관리 API
- MySQL을 활용한 관계형 데이터베이스 구성

## 기술 스택

- Framework: FastAPI (Python)
- Database: MySQL
- ORM: SQLAlchemy
- 인증: JWT, bcrypt
- 기타: Docker, Alembic (선택사항)

## 요구사항 정리

1. 유저는 id, password로 회원가입할 수 있습니다.
    - password는 암호화(bcrypt)되어야 합니다.
2. 유저는 로그인/로그아웃할 수 있습니다.
    - 로그인/로그아웃 시 이력이 기록되어야 합니다.
    - 특정 API는 로그인된 유저만 접근 가능합니다. (예: 잔고조회)
3. 유저는 입출금을 할 수 있습니다.
    - 입금 시 원화 잔고 증가
    - 출금 시 원화 잔고 차감
    - 입출금 내역은 모두 기록되어야 합니다.
4. 유저는 자문을 요청할 수 있습니다.
    - 자문 요청 전 위험도를 선택해야 합니다.
    - 유형1: 잔고를 최대한 많이 사용하는 포트폴리오
    - 유형2: 잔고를 중간 정도 사용하는 포트폴리오
5. 포트폴리오 구성을 위한 증권을 관리할 수 있어야 합니다.
    - 증권 속성: 증권코드, 이름, 가격
    - 최소 10개 증권이 등록되어 있어야 합니다.
    - 등록 API, 가격 수정 API, 삭제 API가 있어야 합니다.
6. 자문 요청 시 잔고를 기반으로 포트폴리오가 구성되어야 합니다.
    - 선택한 포트폴리오 유형에 따라 잔고가 사용됩니다.
    - 증권은 정수 단위로만 구성됩니다.

## 구조

```
portfolio-manage-toyproject
├── main.py # FastAPI 앱 실행 및 라우터 등록 
├── database.py # DB 연결 및 세션 관리
│ ├── account/ 
│ │ ├── domain/
│ │ │ └── account.py # 현재 도메인의 모델
│ │ ├── application/
│ │ │ ├── service.py
│ │ │ ├── schemas.py
│ │ │ └── mapper.py
│ │ ├── adapter/
│ │ │ ├── inbound/
│ │ │ │ └── router.py
│ │ │ ├── outbound/
│ │ │ │ ├── db_models.py
│ │ │ └─└── repository.py
│ │ ├── port/
│ │ │ ├── inbound/
│ │ │ │ └── router.py
│ │ │ ├── outbound/
│ │ │ │ ├── db_models.py
│ │ └─└─└── repository.py
├── tests/
│ ├── test_product_service.py
│ └── test_product_api.py
└── README.md # 프로젝트 설명 (본 파일)
```
## 설치 및 실행 방법

```bash
1. 가상환경 생성
python -m venv venv
source venv/bin/activate  # Windows의 경우 venv\Scripts\activate

# 3. 의존성 설치
pip install -r requirements.txt

# 4. DB 설정 (MySQL 실행 또는 Docker 사용)

# 5. 서버 실행
uvicorn app.main:app --reload
```