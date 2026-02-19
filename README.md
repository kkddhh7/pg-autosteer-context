# AutoSteer — Evaluation & Experimentation Context

간단 소개
: 이 저장소는 AutoSteer 관련 연구·실험을 위한 실행 가능한 컨텍스트입니다. 쿼리 최적화, 부하 생성( CPU / I/O ), 벤치마크 실행, 모델 학습/추론 흐름을 포함한 도구와 스크립트를 제공합니다. 라이브러리 배포용이 아닌 연구 재현성과 실험 자동화를 목표로 합니다.

주요 내용
:
- 실험 제어 · 실행 스크립트 (`main.py`, `run.sh`)
- 다양한 DB 커넥터(`connectors/`)와 벤치마크 쿼리(`benchmark/`)
- 부하 생성 및 측정 도구(`burden/`, `load/`, `utils/disk_measurement.py` 등)
- 학습/추론 파이프라인(`inference/`, `nn/`)
- AutoSteer 탐색/옵티마이저 관련 코드(`autosteer/`)

빠른 시작
:
1. 요구사항 설치

```bash
python3 -m pip install -r requirements.txt
```

2. 설정 파일 확인

- 실험/커넥터 설정은 `configs/` 및 `conf/` 아래에 있습니다. 예: `configs/postgres.cfg`, `conf/postgres.param1.conf` 등

3. 간단 실행 예시

- 로컬 Python 실행 (예시)

```bash
python3 main.py --inference --database postgres --benchmark benchmark/queries/tpch
```

- Docker 기반 PostgreSQL 환경에서 전체 흐름을 실행하려면 (환경에 따라 sudo 필요)

```bash
./run.sh
```

구성 요소(간단 설명)
:
- `autosteer/` — 탐색, 힌트 선택, 옵티마이저 설정과 연관된 핵심 연구코드
- `connectors/` — 데이터베이스 연결 드라이버들(duckdb, mysql, postgres, presto, spark 구현체 포함)
- `benchmark/` — TPC-H 등 SQL 벤치마크 쿼리와 스키마
- `burden/`, `load/` — CPU·메모리·I/O 부하 생성 및 측정 유틸리티
- `inference/` — 모델 정의·학습·평가 파이프라인 (예: `train.py`, `performance_prediction.py`)
- `nn/`, `tree_conv/` — 신경망 모델 구현과 트리 컨볼루션 유틸리티
- `evaluation/`, `results/` — 실험 결과와 평가 스크립트, 출력 폴더
- `utils/` — 공용 유틸리티(인자 파싱, 로깅, 파일 I/O 등)
- `configs/`, `conf/` — 실험 및 DB별 구성 파일 모음

설치 및 개발 노트
:
- Python 3.8+ 권장
- DB별로 추가 드라이버(예: `psycopg2` for PostgreSQL, `pymysql` 등)가 필요할 수 있습니다. `requirements.txt`를 먼저 확인하세요.
- Docker 기반 실험은 `Dockerfile.postgres`와 `run.sh`를 참고하세요.

사용 예시(개발자 관점)
:
- 모델 학습

```bash
python3 inference/train.py --config configs/postgres.cfg
```

- 추론/성능 예측

```bash
python3 inference/performance_prediction.py --model nn/postgres_model --query benchmark/queries/tpch/1.sql
```

테스트
:
- 간단한 단위/통합 테스트는 `test/` 폴더를 확인하세요. 예:

```bash
pytest -q
```
