# 🤖 Enhanced AutoGPT - Production Ready System

고급 자기개선 기능을 갖춘 프로덕션 환경용 AutoGPT 시스템입니다.

## 🌟 주요 기능

### 🧠 **LLM 통합**
- **OpenAI GPT-4** 및 **Anthropic Claude** API 지원
- 지능적 목표 분해 및 작업 계획 수립
- 컨텍스트 기반 작업 최적화

### 🔧 **자기개선 시스템**
- 성능 분석 및 학습 패턴 저장
- 실시간 피드백 기반 최적화
- 작업 성공률 및 실행시간 개선

### 🌐 **웹 인터페이스**
- **React** 기반 현대적 UI
- 실시간 WebSocket 업데이트
- 대시보드, 목표 관리, 작업 모니터링

### ⚡ **분산 처리**
- **Celery + Redis** 기반 확장 가능한 작업 큐
- 병렬 작업 처리
- 자동 재시도 및 오류 복구

### 🔒 **보안 강화**
- **Docker** 기반 샌드박스 코드 실행
- 제한된 권한으로 안전한 작업 수행
- 네트워크 격리 및 리소스 제한

### 📊 **모니터링**
- **Prometheus + Grafana** 기반 실시간 모니터링
- 시스템 메트릭, 성능 지표
- 알림 및 대시보드

## 🚀 빠른 시작

### 1. 저장소 클론
```bash
git clone <repository-url>
cd enhanced-autogpt
```

### 2. 개발 환경 설정
```bash
chmod +x scripts/*.sh
./scripts/setup.sh
```

### 3. 환경 변수 설정
`.env` 파일을 편집하여 API 키를 설정하세요:
```bash
nano .env
```

```env
# API Keys - 반드시 설정 필요
OPENAI_API_KEY=sk-your-openai-key-here
ANTHROPIC_API_KEY=sk-ant-your-anthropic-key-here

# 나머지는 기본값 사용 가능
DATABASE_URL=postgresql://autogpt:autogpt_password@postgres:5432/autogpt
REDIS_URL=redis://redis:6379/0
```

### 4. 프로덕션 배포
```bash
./scripts/deploy.sh
```

### 5. 서비스 접속
- **메인 애플리케이션**: http://localhost
- **API 문서**: http://localhost/api/docs
- **Grafana 대시보드**: http://localhost/monitoring (admin/admin123)
- **Prometheus**: http://localhost:9090

## 📋 시스템 요구사항

### 최소 요구사항
- **OS**: Linux, macOS, Windows (WSL2)
- **Docker**: 20.10+ 및 Docker Compose
- **메모리**: 4GB RAM
- **디스크**: 10GB 여유 공간
- **네트워크**: 인터넷 연결 (API 호출용)

### 권장 사양
- **메모리**: 8GB+ RAM
- **CPU**: 4+ 코어
- **디스크**: SSD 20GB+

## 🛠️ 개발 모드

### Python 백엔드 개발
```bash
# 가상환경 활성화
source venv/bin/activate

# 개발 서버 시작
python main.py

# 테스트 실행
pytest tests/

# 코드 포맷팅
black .
flake8 .
```

### React 프론트엔드 개발
```bash
cd frontend
npm start  # http://localhost:3000
```

### Celery 워커 개발
```bash
# 별도 터미널에서
celery -A main.celery_app worker --loglevel=info
```

## 📊 사용법

### 1. 목표 설정
```python
# Python API 사용
import requests

response = requests.post('http://localhost:8000/api/goals', json={
    'description': 'Python 웹 크롤러 개발',
    'success_criteria': [
        '기능적인 크롤러 코드 생성',
        '테스트 코드 작성',
        '문서화 완료'
    ],
    'priority': 1,
    'use_llm': True
})
```

### 2. 웹 인터페이스 사용
1. http://localhost 접속
2. "새 목표 추가" 버튼 클릭
3. 목표 설명 및 성공 기준 입력
4. LLM 자동 분해 옵션 선택
5. 실시간으로 진행 상황 모니터링

### 3. API 직접 사용
```bash
# 목표 생성
curl -X POST "http://localhost:8000/api/goals" \
  -H "Content-Type: application/json" \
  -d '{
    "description": "시장 분석 보고서 작성",
    "success_criteria": ["데이터 수집", "분석 수행", "보고서 생성"],
    "priority": 2
  }'

# 목표 목록 조회
curl "http://localhost:8000/api/goals"

# 시스템 상태 확인
curl "http://localhost:8000/api/status"
```

## 🔧 설정 옵션

### 환경 변수 설명

| 변수 | 설명 | 기본값 |
|------|------|--------|
| `OPENAI_API_KEY` | OpenAI API 키 | - |
| `ANTHROPIC_API_KEY` | Anthropic API 키 | - |
| `DATABASE_URL` | 데이터베이스 연결 URL | `sqlite:///autogpt.db` |
| `REDIS_URL` | Redis 연결 URL | `redis://localhost:6379/0` |
| `ENABLE_SANDBOX` | 샌드박스 활성화 | `true` |
| `WEB_PORT` | 웹 서버 포트 | `8000` |
| `DOCKER_SANDBOX_IMAGE` | 샌드박스용 Docker 이미지 | `python:3.11-slim` |

### 고급 설정

#### Celery 큐 설정
```python
# main.py에서 수정
celery_app.conf.update(
    task_routes={
        "execute_task": {"queue": "tasks"},
        "analyze_performance": {"queue": "analysis"},
        "llm_process": {"queue": "llm"}
    },
    worker_prefetch_multiplier=1,
    task_acks_late=True,
)
```

#### 샌드박스 리소스 제한
```python
# main.py SecuritySandbox 클래스에서 수정
container = self.docker_client.containers.run(
    config.DOCKER_SANDBOX_IMAGE,
    mem_limit="256m",      # 메모리 제한
    cpu_quota=100000,      # CPU 제한
    timeout=60             # 실행 시간 제한
)
```

## 📈 모니터링

### Grafana 대시보드
- **시스템 메트릭**: CPU, 메모리, 디스크 사용률
- **작업 메트릭**: 성공률, 실행 시간, 큐 길이
- **비즈니스 메트릭**: 목표 완료율, 학습 패턴 수

### 알림 설정
```yaml
# monitoring/alert_rules.yml 수정
- alert: HighTaskFailureRate
  expr: rate(autogpt_tasks_total{status="failed"}[5m]) > 0.1
  for: 2m
  labels:
    severity: critical
  annotations:
    summary: "작업 실패율이 높습니다"
```

### 로그 모니터링
```bash
# 실시간 로그 확인
docker-compose logs -f autogpt-api

# 특정 서비스 로그
docker-compose logs celery-worker

# 에러 로그만 필터링
docker-compose logs autogpt-api | grep ERROR
```

## 🛡️ 보안 고려사항

### API 키 보안
- 환경 변수로만 API 키 관리
- `.env` 파일을 Git에 커밋하지 마세요
- 프로덕션에서는 Key Management Service 사용 권장

### Docker 보안
```dockerfile
# 비root 사용자로 실행
RUN adduser --disabled-password --gecos '' appuser
USER appuser

# 읽기 전용 파일시스템
docker run --read-only ...

# 네트워크 제한
docker run --network none ...
```

### 데이터베이스 보안
- 강력한 비밀번호 사용
- SSL/TLS 연결 활성화
- 정기적인 백업 수행

## 🔄 백업 및 복구

### 자동 백업
```bash
# 일일 백업 스크립트 실행
./scripts/backup.sh

# cron으로 자동화
0 2 * * * /path/to/enhanced-autogpt/scripts/backup.sh
```

### 데이터 복구
```bash
# PostgreSQL 복구
docker-compose exec postgres psql -U autogpt -d autogpt < backup.sql

# Redis 복구
docker-compose exec redis redis-cli --rdb backup.rdb
```

## 🐛 문제 해결

### 일반적인 문제

#### 1. Docker 권한 오류
```bash
# Docker 그룹에 사용자 추가
sudo usermod -aG docker $USER
# 재로그인 필요
```

#### 2. 포트 충돌
```bash
# 사용 중인 포트 확인
netstat -tulpn | grep :8000

# docker-compose.yml에서 포트 변경
ports:
  - "8001:8000"  # 외부:내부
```

#### 3. 메모리 부족
```bash
# 메모리 사용량 확인
docker stats

# 불필요한 컨테이너 정리
docker system prune -a
```

#### 4. API 키 오류
```bash
# 환경 변수 확인
echo $OPENAI_API_KEY

# 컨테이너 내부 확인
docker-compose exec autogpt-api env | grep API_KEY
```

### 디버깅

#### 로그 레벨 조정
```python
# main.py에서
logging.basicConfig(level=logging.DEBUG)
```

#### 데이터베이스 확인
```bash
# PostgreSQL 접속
docker-compose exec postgres psql -U autogpt

# 테이블 확인
\dt

# 데이터 조회
SELECT * FROM goals LIMIT 5;
```

#### Redis 상태 확인
```bash
# Redis CLI 접속
docker-compose exec redis redis-cli

# 키 목록 확인
KEYS *

# 큐 상태 확인
LLEN celery
```

## 🔮 로드맵

### v2.1 (계획 중)
- [ ] 다중 LLM 모델 지원 (GPT-4, Claude, Gemini)
- [ ] 음성 인터페이스 (STT/TTS)
- [ ] 플러그인 시스템

### v2.2 (계획 중)
- [ ] 분산 멀티노드 처리
- [ ] 블록체인 기반 작업 검증
- [ ] AI 모델 파인튜닝

### v3.0 (계획 중)
- [ ] 완전 자율적 목표 설정
- [ ] 멀티모달 입출력 지원
- [ ] 연합학습 기능

## 🤝 기여하기

### 개발 참여
1. Fork 저장소
2. 기능 브랜치 생성 (`git checkout -b feature/amazing-feature`)
3. 변경사항 커밋 (`git commit -m 'Add amazing feature'`)
4. 브랜치에 Push (`git push origin feature/amazing-feature`)
5. Pull Request 생성

### 코드 스타일
- Python: Black + Flake8
- JavaScript: Prettier + ESLint
- 커밋 메시지: Conventional Commits

### 테스트
```bash
# Python 테스트
pytest tests/ --cov=.

# JavaScript 테스트
cd frontend && npm test
```

## 📄 라이선스

MIT License - 자세한 내용은 [LICENSE](LICENSE) 파일을 참조하세요.

## 🙏 감사의 말

- OpenAI GPT-4
- Anthropic Claude
- FastAPI, React, Celery 등 오픈소스 커뮤니티

## 📞 지원

- **이슈**: GitHub Issues
- **문서**: [Wiki](wiki)
- **토론**: [Discussions](discussions)

---

**⚠️ 주의사항**: 이 시스템은 강력한 자동화 도구입니다. 프로덕션 환경에서 사용하기 전에 충분한 테스트를 수행하고, 적절한 보안 조치를 취하세요.
