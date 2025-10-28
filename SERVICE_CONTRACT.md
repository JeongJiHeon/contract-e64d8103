# 자동 요약/태깅 서비스 계약서

## 문서 정보
- **프로젝트명**: 자동 요약/태깅 서비스 (Auto Summary & Tagging Service)
- **버전**: 1.0.0
- **작성일**: 2025-10-28
- **문서 상태**: 초안

---

## 1. 서비스 개요

### 1.1 서비스명 및 설명
**서비스명**: AI 기반 자동 요약 및 태깅 플랫폼

**서비스 설명**:
사용자가 제공한 텍스트, 문서, URL 링크 등의 콘텐츠를 AI 기술을 활용하여 자동으로 요약하고, 관련 태그를 생성하는 지능형 서비스입니다. 복잡하고 긴 문서를 빠르게 이해하고, 효율적으로 정보를 관리할 수 있도록 지원합니다.

### 1.2 서비스 목표
- **생산성 향상**: 긴 문서를 읽는 시간을 70% 이상 단축
- **정보 관리 효율화**: 자동 태깅을 통한 콘텐츠 분류 및 검색 개선
- **AI 활용**: 최신 LLM 기술을 활용한 정확하고 자연스러운 요약 제공
- **사용자 경험**: 직관적이고 빠른 인터페이스 제공 (응답 시간 < 3초)

### 1.3 타겟 사용자
- **연구원 및 학생**: 논문, 학술 자료 요약
- **비즈니스 전문가**: 보고서, 뉴스, 시장 분석 자료 요약
- **콘텐츠 크리에이터**: 블로그, 기사 관리 및 분류
- **일반 사용자**: 웹 아티클, 블로그 포스트 빠른 이해

### 1.4 핵심 기능
1. **텍스트 요약**
   - 직접 입력 텍스트 요약
   - 파일 업로드 (PDF, DOCX, TXT) 지원
   - 요약 길이 조절 (짧게, 중간, 길게)
   - 다국어 지원 (한국어, 영어 우선)

2. **자동 태깅**
   - 콘텐츠 분석 기반 태그 자동 생성
   - 카테고리 분류
   - 키워드 추출
   - 사용자 정의 태그 추가/수정

3. **URL 크롤링 및 요약**
   - 웹 페이지 콘텐츠 자동 추출
   - 메타데이터 수집
   - 본문 추출 및 요약

4. **히스토리 관리**
   - 요약 이력 저장
   - 즐겨찾기 기능
   - 검색 및 필터링
   - 태그 기반 정리

---

## 2. 시스템 아키텍처

### 2.1 전체 시스템 구조

```mermaid
graph TB
    subgraph "Frontend Layer"
        A[React Web App]
        B[State Management: Redux Toolkit]
    end
    
    subgraph "API Gateway"
        C[NGINX Reverse Proxy]
    end
    
    subgraph "Backend Layer"
        D[FastAPI Server]
        E[Background Worker: Celery]
    end
    
    subgraph "Agent Layer"
        F[LangChain Agent]
        G[LLM: OpenAI GPT-4]
        H[Web Scraper Tool]
        I[Document Parser Tool]
    end
    
    subgraph "Data Layer"
        J[(PostgreSQL)]
        K[(Redis Cache)]
        L[S3 Storage]
    end
    
    A -->|HTTPS| C
    C -->|REST API| D
    D -->|Queue Tasks| E
    E -->|Process| F
    F -->|LLM Requests| G
    F -->|Scrape| H
    F -->|Parse| I
    D -->|Query/Store| J
    D -->|Cache| K
    D -->|Upload Files| L
    E -->|Store Results| J
```

### 2.2 컴포넌트 개요

#### Frontend (프론트엔드)
- **역할**: 사용자 인터페이스 및 상호작용
- **주요 기능**: 콘텐츠 입력, 요약 결과 표시, 히스토리 관리

#### Backend (백엔드)
- **역할**: API 서버, 비즈니스 로직, 데이터 관리
- **주요 기능**: 인증, API 엔드포인트, 작업 큐 관리

#### Agent (에이전트)
- **역할**: AI 기반 요약 및 태깅 수행
- **주요 기능**: LLM 호출, 문서 파싱, 웹 스크래핑

### 2.3 기술 스택

#### Frontend
| 기술 | 버전 | 용도 |
|------|------|------|
| React | 18.2.0 | UI 프레임워크 |
| TypeScript | 5.0+ | 타입 안정성 |
| Redux Toolkit | 1.9+ | 상태 관리 |
| React Router | 6.11+ | 라우팅 |
| Axios | 1.4+ | HTTP 클라이언트 |
| TailwindCSS | 3.3+ | 스타일링 |
| React Query | 4.29+ | 서버 상태 관리 |

#### Backend
| 기술 | 버전 | 용도 |
|------|------|------|
| Python | 3.11+ | 프로그래밍 언어 |
| FastAPI | 0.104+ | 웹 프레임워크 |
| PostgreSQL | 15+ | 메인 데이터베이스 |
| Redis | 7.2+ | 캐싱 & 세션 |
| Celery | 5.3+ | 비동기 작업 큐 |
| SQLAlchemy | 2.0+ | ORM |
| Pydantic | 2.0+ | 데이터 검증 |
| Alembic | 1.12+ | 데이터베이스 마이그레이션 |

#### Agent & AI
| 기술 | 버전 | 용도 |
|------|------|------|
| LangChain | 0.1.0+ | 에이전트 프레임워크 |
| OpenAI API | GPT-4 | LLM |
| BeautifulSoup4 | 4.12+ | HTML 파싱 |
| PyPDF2 | 3.0+ | PDF 파싱 |
| python-docx | 1.0+ | DOCX 파싱 |
| tiktoken | 0.5+ | 토큰 카운팅 |

#### Infrastructure
| 기술 | 버전 | 용도 |
|------|------|------|
| Docker | 24.0+ | 컨테이너화 |
| Docker Compose | 2.20+ | 로컬 개발 환경 |
| NGINX | 1.25+ | 리버스 프록시 |
| AWS S3 | - | 파일 저장소 |
| AWS RDS | - | 데이터베이스 호스팅 |
| AWS ElastiCache | - | Redis 호스팅 |

---

## 3. 프론트엔드 사양

### 3.1 페이지 구조

```mermaid
graph LR
    A[Landing Page] --> B[Sign Up / Login]
    B --> C[Dashboard]
    C --> D[New Summary]
    C --> E[History]
    C --> F[Settings]
    D --> G[Summary Result]
    E --> G
```

### 3.2 주요 페이지

#### 3.2.1 랜딩 페이지 (`/`)
- **목적**: 서비스 소개 및 회원가입/로그인 유도
- **컴포넌트**:
  - Hero Section: 서비스 가치 제안
  - Features Section: 핵심 기능 소개
  - Pricing Section: 요금제 안내
  - CTA Buttons: 회원가입/로그인 버튼

#### 3.2.2 대시보드 (`/dashboard`)
- **목적**: 메인 작업 공간
- **컴포넌트**:
  - Navigation Bar
  - Quick Action Panel: 새 요약 생성
  - Recent Summaries Widget
  - Usage Statistics
  - Tag Cloud

#### 3.2.3 새 요약 생성 (`/dashboard/new`)
- **목적**: 콘텐츠 입력 및 요약 요청
- **컴포넌트**:
  - Input Type Selector (텍스트/파일/URL)
  - Text Area / File Upload / URL Input
  - Options Panel:
    - 요약 길이 선택 (짧게/중간/길게)
    - 언어 선택
    - 태그 자동 생성 여부
  - Submit Button
  - Loading Indicator

#### 3.2.4 요약 결과 (`/dashboard/summary/:id`)
- **목적**: 요약 결과 표시 및 관리
- **컴포넌트**:
  - Original Content Preview
  - Summary Display
  - Generated Tags (편집 가능)
  - Action Buttons:
    - 복사하기
    - 다운로드 (PDF/TXT)
    - 공유하기
    - 즐겨찾기
  - Feedback Section (좋아요/별로예요)

#### 3.2.5 히스토리 (`/dashboard/history`)
- **목적**: 과거 요약 이력 조회 및 관리
- **컴포넌트**:
  - Search Bar
  - Filter Panel (날짜, 태그, 타입)
  - Summary Cards Grid
  - Pagination
  - Bulk Actions (삭제, 내보내기)

#### 3.2.6 설정 (`/dashboard/settings`)
- **목적**: 사용자 설정 관리
- **컴포넌트**:
  - Profile Settings
  - API Key Management
  - Default Preferences
  - Notification Settings

### 3.3 컴포넌트 계층 구조

```
App
├── Router
│   ├── PublicLayout
│   │   ├── Header
│   │   ├── LandingPage
│   │   └── Footer
│   ├── AuthLayout
│   │   ├── LoginPage
│   │   └── SignUpPage
│   └── PrivateLayout
│       ├── Navigation
│       ├── Dashboard
│       │   ├── QuickAction
│       │   ├── RecentSummaries
│       │   └── Statistics
│       ├── NewSummary
│       │   ├── InputSelector
│       │   ├── TextInput
│       │   ├── FileUpload
│       │   ├── URLInput
│       │   └── OptionsPanel
│       ├── SummaryResult
│       │   ├── ContentPreview
│       │   ├── SummaryDisplay
│       │   ├── TagEditor
│       │   └── ActionBar
│       ├── History
│       │   ├── SearchBar
│       │   ├── FilterPanel
│       │   ├── SummaryGrid
│       │   └── Pagination
│       └── Settings
│           ├── ProfileForm
│           ├── APIKeyManager
│           └── PreferencesForm
```

### 3.4 상태 관리

#### Redux Store 구조
```typescript
{
  auth: {
    user: User | null,
    token: string | null,
    isAuthenticated: boolean
  },
  summaries: {
    items: Summary[],
    currentSummary: Summary | null,
    loading: boolean,
    error: string | null
  },
  ui: {
    sidebarOpen: boolean,
    theme: 'light' | 'dark',
    notifications: Notification[]
  }
}
```

### 3.5 API 통신 패턴

#### Axios 인스턴스 설정
```typescript
// api/client.ts
const apiClient = axios.create({
  baseURL: process.env.REACT_APP_API_URL,
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json'
  }
});

// Request Interceptor: 토큰 추가
apiClient.interceptors.request.use((config) => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Response Interceptor: 에러 핸들링
apiClient.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      // 로그아웃 처리
      store.dispatch(logout());
    }
    return Promise.reject(error);
  }
);
```

### 3.6 UI/UX 요구사항

1. **반응형 디자인**: 모바일, 태블릿, 데스크톱 지원
2. **접근성**: WCAG 2.1 AA 레벨 준수
3. **성능**:
   - First Contentful Paint < 1.5초
   - Time to Interactive < 3초
   - Lighthouse Score > 90
4. **다크 모드**: 시스템 설정 연동
5. **인터랙션**:
   - 로딩 상태 명확히 표시
   - 에러 메시지 사용자 친화적
   - 성공 피드백 (토스트 알림)

---

## 4. 백엔드 사양

### 4.1 시스템 구성

```mermaid
graph TB
    A[NGINX] --> B[FastAPI App]
    B --> C[Authentication Middleware]
    B --> D[Rate Limiting Middleware]
    B --> E[API Routes]
    E --> F[Service Layer]
    F --> G[Repository Layer]
    G --> H[(PostgreSQL)]
    F --> I[Celery Tasks]
    I --> J[Agent Worker]
    F --> K[(Redis)]
    B --> L[Error Handler]
```

### 4.2 프로젝트 구조

```
backend/
├── app/
│   ├── __init__.py
│   ├── main.py                 # FastAPI 애플리케이션 진입점
│   ├── config.py               # 설정 관리
│   ├── dependencies.py         # 의존성 주입
│   │
│   ├── api/                    # API 엔드포인트
│   │   ├── __init__.py
│   │   ├── auth.py
│   │   ├── summaries.py
│   │   ├── users.py
│   │   └── tags.py
│   │
│   ├── models/                 # SQLAlchemy 모델
│   │   ├── __init__.py
│   │   ├── user.py
│   │   ├── summary.py
│   │   └── tag.py
│   │
│   ├── schemas/                # Pydantic 스키마
│   │   ├── __init__.py
│   │   ├── user.py
│   │   ├── summary.py
│   │   └── tag.py
│   │
│   ├── services/               # 비즈니스 로직
│   │   ├── __init__.py
│   │   ├── auth_service.py
│   │   ├── summary_service.py
│   │   └── user_service.py
│   │
│   ├── repositories/           # 데이터 액세스
│   │   ├── __init__.py
│   │   ├── user_repository.py
│   │   └── summary_repository.py
│   │
│   ├── core/                   # 핵심 유틸리티
│   │   ├── __init__.py
│   │   ├── security.py         # JWT, 암호화
│   │   ├── database.py         # DB 연결
│   │   └── cache.py            # Redis 연결
│   │
│   ├── tasks/                  # Celery 작업
│   │   ├── __init__.py
│   │   ├── celery_app.py
│   │   └── summary_tasks.py
│   │
│   └── utils/                  # 유틸리티 함수
│       ├── __init__.py
│       ├── file_handler.py
│       └── validators.py
│
├── tests/                      # 테스트
│   ├── __init__.py
│   ├── test_api/
│   └── test_services/
│
├── alembic/                    # DB 마이그레이션
│   ├── versions/
│   └── env.py
│
├── requirements.txt
├── Dockerfile
└── docker-compose.yml
```

### 4.3 데이터베이스 스키마

#### Users 테이블
```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    username VARCHAR(100) UNIQUE NOT NULL,
    hashed_password VARCHAR(255) NOT NULL,
    full_name VARCHAR(255),
    is_active BOOLEAN DEFAULT TRUE,
    is_superuser BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    
    CONSTRAINT email_format CHECK (email ~* '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}$')
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_username ON users(username);
```

#### Summaries 테이블
```sql
CREATE TABLE summaries (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    title VARCHAR(500),
    original_content TEXT NOT NULL,
    summary_content TEXT,
    input_type VARCHAR(20) NOT NULL,  -- 'text', 'file', 'url'
    source_url VARCHAR(2048),
    file_path VARCHAR(500),
    language VARCHAR(10) DEFAULT 'ko',
    summary_length VARCHAR(20),  -- 'short', 'medium', 'long'
    status VARCHAR(20) NOT NULL DEFAULT 'pending',  -- 'pending', 'processing', 'completed', 'failed'
    processing_time_ms INTEGER,
    token_count INTEGER,
    is_favorite BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    
    CONSTRAINT input_type_check CHECK (input_type IN ('text', 'file', 'url')),
    CONSTRAINT status_check CHECK (status IN ('pending', 'processing', 'completed', 'failed')),
    CONSTRAINT summary_length_check CHECK (summary_length IN ('short', 'medium', 'long'))
);

CREATE INDEX idx_summaries_user_id ON summaries(user_id);
CREATE INDEX idx_summaries_status ON summaries(status);
CREATE INDEX idx_summaries_created_at ON summaries(created_at DESC);
CREATE INDEX idx_summaries_is_favorite ON summaries(user_id, is_favorite) WHERE is_favorite = TRUE;
```

#### Tags 테이블
```sql
CREATE TABLE tags (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL,
    color VARCHAR(7),  -- HEX color code
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    
    UNIQUE(name)
);

CREATE INDEX idx_tags_name ON tags(name);
```

#### Summary_Tags 테이블 (다대다 관계)
```sql
CREATE TABLE summary_tags (
    summary_id UUID NOT NULL REFERENCES summaries(id) ON DELETE CASCADE,
    tag_id UUID NOT NULL REFERENCES tags(id) ON DELETE CASCADE,
    is_auto_generated BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    
    PRIMARY KEY (summary_id, tag_id)
);

CREATE INDEX idx_summary_tags_summary_id ON summary_tags(summary_id);
CREATE INDEX idx_summary_tags_tag_id ON summary_tags(tag_id);
```

#### User_Feedback 테이블
```sql
CREATE TABLE user_feedback (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    summary_id UUID NOT NULL REFERENCES summaries(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    rating INTEGER,  -- 1-5
    feedback_type VARCHAR(20),  -- 'positive', 'negative'
    comment TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    
    CONSTRAINT rating_check CHECK (rating >= 1 AND rating <= 5),
    CONSTRAINT feedback_type_check CHECK (feedback_type IN ('positive', 'negative')),
    UNIQUE(summary_id, user_id)
);

CREATE INDEX idx_user_feedback_summary_id ON user_feedback(summary_id);
```

### 4.4 인증 및 인가

#### JWT 토큰 구조
```python
# core/security.py
from datetime import datetime, timedelta
from jose import JWTError, jwt
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

SECRET_KEY = "your-secret-key-here"  # 환경 변수로 관리
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30
REFRESH_TOKEN_EXPIRE_DAYS = 7

def create_access_token(data: dict, expires_delta: timedelta = None):
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    
    to_encode.update({"exp": expire, "type": "access"})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt

def verify_password(plain_password: str, hashed_password: str) -> bool:
    return pwd_context.verify(plain_password, hashed_password)

def get_password_hash(password: str) -> str:
    return pwd_context.hash(password)
```

#### 인증 의존성
```python
# dependencies.py
from fastapi import Depends, HTTPException, status
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
from jose import JWTError, jwt

security = HTTPBearer()

async def get_current_user(
    credentials: HTTPAuthorizationCredentials = Depends(security),
    db: Session = Depends(get_db)
) -> User:
    try:
        payload = jwt.decode(
            credentials.credentials, 
            SECRET_KEY, 
            algorithms=[ALGORITHM]
        )
        user_id: str = payload.get("sub")
        if user_id is None:
            raise HTTPException(
                status_code=status.HTTP_401_UNAUTHORIZED,
                detail="Could not validate credentials"
            )
    except JWTError:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Could not validate credentials"
        )
    
    user = db.query(User).filter(User.id == user_id).first()
    if user is None:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```

### 4.5 캐싱 전략

#### Redis 캐시 레이어
```python
# core/cache.py
import redis
import json
from typing import Optional, Any
from datetime import timedelta

redis_client = redis.Redis(
    host='localhost',
    port=6379,
    db=0,
    decode_responses=True
)

class CacheService:
    @staticmethod
    def get(key: str) -> Optional[Any]:
        value = redis_client.get(key)
        if value:
            return json.loads(value)
        return None
    
    @staticmethod
    def set(key: str, value: Any, expire: int = 3600):
        redis_client.setex(
            key,
            expire,
            json.dumps(value, default=str)
        )
    
    @staticmethod
    def delete(key: str):
        redis_client.delete(key)
    
    @staticmethod
    def get_summary(summary_id: str) -> Optional[dict]:
        return CacheService.get(f"summary:{summary_id}")
    
    @staticmethod
    def set_summary(summary_id: str, summary_data: dict):
        CacheService.set(f"summary:{summary_id}", summary_data, expire=1800)
```

#### 캐싱 대상
1. **요약 결과**: 30분 TTL
2. **사용자 프로필**: 1시간 TTL
3. **태그 목록**: 24시간 TTL
4. **API Rate Limit 카운터**: 1분 TTL

### 4.6 백그라운드 작업 (Celery)

#### Celery 설정
```python
# tasks/celery_app.py
from celery import Celery

celery_app = Celery(
    'summary_tasks',
    broker='redis://localhost:6379/0',
    backend='redis://localhost:6379/0'
)

celery_app.conf.update(
    task_serializer='json',
    accept_content=['json'],
    result_serializer='json',
    timezone='Asia/Seoul',
    enable_utc=True,
    task_track_started=True,
    task_time_limit=300,  # 5분
    task_soft_time_limit=240,  # 4분
)
```

#### 요약 생성 작업
```python
# tasks/summary_tasks.py
from tasks.celery_app import celery_app
from agent.summary_agent import SummaryAgent

@celery_app.task(bind=True)
def process_summary_task(self, summary_id: str, content: str, options: dict):
    """
    비동기로 요약 생성 작업 수행
    """
    try:
        # DB에서 Summary 레코드 조회
        summary = get_summary_by_id(summary_id)
        summary.status = 'processing'
        save_summary(summary)
        
        # Agent를 통해 요약 생성
        agent = SummaryAgent()
        result = agent.summarize(
            content=content,
            length=options.get('length', 'medium'),
            language=options.get('language', 'ko')
        )
        
        # 태그 생성
        tags = agent.generate_tags(content, result['summary'])
        
        # 결과 저장
        summary.summary_content = result['summary']
        summary.token_count = result['token_count']
        summary.processing_time_ms = result['processing_time_ms']
        summary.status = 'completed'
        save_summary(summary)
        
        # 태그 저장
        save_tags_for_summary(summary_id, tags)
        
        return {'status': 'success', 'summary_id': summary_id}
        
    except Exception as e:
        summary.status = 'failed'
        save_summary(summary)
        raise self.retry(exc=e, countdown=60, max_retries=3)
```

---

## 5. API 사양

### 5.1 API 엔드포인트 목록

#### 인증 (Authentication)
| 메서드 | 엔드포인트 | 설명 | 인증 필요 |
|--------|-----------|------|----------|
| POST | `/api/v1/auth/register` | 회원가입 | ❌ |
| POST | `/api/v1/auth/login` | 로그인 | ❌ |
| POST | `/api/v1/auth/refresh` | 토큰 갱신 | ✅ |
| POST | `/api/v1/auth/logout` | 로그아웃 | ✅ |
| GET | `/api/v1/auth/me` | 현재 사용자 정보 | ✅ |

#### 요약 (Summaries)
| 메서드 | 엔드포인트 | 설명 | 인증 필요 |
|--------|-----------|------|----------|
| POST | `/api/v1/summaries` | 새 요약 생성 | ✅ |
| GET | `/api/v1/summaries` | 요약 목록 조회 | ✅ |
| GET | `/api/v1/summaries/{id}` | 요약 상세 조회 | ✅ |
| PUT | `/api/v1/summaries/{id}` | 요약 수정 | ✅ |
| DELETE | `/api/v1/summaries/{id}` | 요약 삭제 | ✅ |
| POST | `/api/v1/summaries/{id}/favorite` | 즐겨찾기 토글 | ✅ |
| GET | `/api/v1/summaries/{id}/status` | 처리 상태 조회 | ✅ |

#### 태그 (Tags)
| 메서드 | 엔드포인트 | 설명 | 인증 필요 |
|--------|-----------|------|----------|
| GET | `/api/v1/tags` | 태그 목록 조회 | ✅ |
| POST | `/api/v1/tags` | 새 태그 생성 | ✅ |
| PUT | `/api/v1/summaries/{id}/tags` | 요약 태그 수정 | ✅ |

#### 사용자 (Users)
| 메서드 | 엔드포인트 | 설명 | 인증 필요 |
|--------|-----------|------|----------|
| GET | `/api/v1/users/me` | 내 프로필 조회 | ✅ |
| PUT | `/api/v1/users/me` | 내 프로필 수정 | ✅ |
| GET | `/api/v1/users/me/stats` | 내 사용 통계 | ✅ |

### 5.2 API 상세 스펙

#### 5.2.1 POST `/api/v1/auth/register` - 회원가입

**Request:**
```json
{
  "email": "user@example.com",
  "username": "johndoe",
  "password": "SecurePass123!",
  "full_name": "John Doe"
}
```

**Response (201 Created):**
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "email": "user@example.com",
  "username": "johndoe",
  "full_name": "John Doe",
  "is_active": true,
  "created_at": "2025-10-28T10:00:00Z"
}
```

**Validation Rules:**
- `email`: 유효한 이메일 형식, 중복 불가
- `username`: 4-20자, 영문/숫자/언더스코어만, 중복 불가
- `password`: 최소 8자, 대소문자/숫자/특수문자 포함

**Error Responses:**
- `400 Bad Request`: 유효성 검증 실패
- `409 Conflict`: 이메일 또는 사용자명 중복

---

#### 5.2.2 POST `/api/v1/auth/login` - 로그인

**Request:**
```json
{
  "email": "user@example.com",
  "password": "SecurePass123!"
}
```

**Response (200 OK):**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "bearer",
  "expires_in": 1800,
  "user": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@example.com",
    "username": "johndoe"
  }
}
```

**Error Responses:**
- `401 Unauthorized`: 잘못된 인증 정보
- `403 Forbidden`: 계정 비활성화

---

#### 5.2.3 POST `/api/v1/summaries` - 새 요약 생성

**Headers:**
```
Authorization: Bearer {access_token}
Content-Type: application/json
```

**Request (텍스트 입력):**
```json
{
  "input_type": "text",
  "content": "긴 텍스트 내용...",
  "title": "문서 제목 (선택)",
  "options": {
    "summary_length": "medium",
    "language": "ko",
    "auto_tag": true
  }
}
```

**Request (URL 입력):**
```json
{
  "input_type": "url",
  "source_url": "https://example.com/article",
  "options": {
    "summary_length": "short",
    "language": "ko",
    "auto_tag": true
  }
}
```

**Request (파일 업로드):**
```
POST /api/v1/summaries
Content-Type: multipart/form-data

file: [binary]
input_type: file
options: {"summary_length": "long", "language": "en", "auto_tag": true}
```

**Response (202 Accepted):**
```json
{
  "id": "660e8400-e29b-41d4-a716-446655440000",
  "status": "pending",
  "message": "Summary generation started",
  "estimated_time_seconds": 15
}
```

**Error Responses:**
- `400 Bad Request`: 잘못된 요청 형식
- `413 Payload Too Large`: 파일 크기 초과 (최대 10MB)
- `429 Too Many Requests`: Rate limit 초과

---

#### 5.2.4 GET `/api/v1/summaries/{id}` - 요약 상세 조회

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response (200 OK):**
```json
{
  "id": "660e8400-e29b-41d4-a716-446655440000",
  "user_id": "550e8400-e29b-41d4-a716-446655440000",
  "title": "AI의 미래에 대한 분석",
  "original_content": "원본 텍스트...",
  "summary_content": "요약된 내용...",
  "input_type": "text",
  "source_url": null,
  "language": "ko",
  "summary_length": "medium",
  "status": "completed",
  "processing_time_ms": 2340,
  "token_count": 450,
  "is_favorite": false,
  "tags": [
    {
      "id": "770e8400-e29b-41d4-a716-446655440000",
      "name": "AI",
      "color": "#3B82F6",
      "is_auto_generated": true
    },
    {
      "id": "880e8400-e29b-41d4-a716-446655440000",
      "name": "기술",
      "color": "#10B981",
      "is_auto_generated": true
    }
  ],
  "created_at": "2025-10-28T10:00:00Z",
  "updated_at": "2025-10-28T10:00:15Z"
}
```

**Error Responses:**
- `404 Not Found`: 요약을 찾을 수 없음
- `403 Forbidden`: 접근 권한 없음

---

#### 5.2.5 GET `/api/v1/summaries` - 요약 목록 조회

**Headers:**
```
Authorization: Bearer {access_token}
```

**Query Parameters:**
- `page` (int, default: 1): 페이지 번호
- `page_size` (int, default: 20, max: 100): 페이지 크기
- `search` (string): 제목/내용 검색
- `tags` (string): 태그 필터 (쉼표로 구분)
- `status` (string): 상태 필터
- `is_favorite` (boolean): 즐겨찾기 필터
- `sort_by` (string, default: "created_at"): 정렬 기준
- `order` (string, default: "desc"): 정렬 순서

**Example Request:**
```
GET /api/v1/summaries?page=1&page_size=20&tags=AI,기술&is_favorite=true&sort_by=created_at&order=desc
```

**Response (200 OK):**
```json
{
  "items": [
    {
      "id": "660e8400-e29b-41d4-a716-446655440000",
      "title": "AI의 미래",
      "summary_content": "요약...",
      "status": "completed",
      "is_favorite": true,
      "tags": ["AI", "기술"],
      "created_at": "2025-10-28T10:00:00Z"
    }
  ],
  "total": 45,
  "page": 1,
  "page_size": 20,
  "total_pages": 3
}
```

---

#### 5.2.6 PUT `/api/v1/summaries/{id}` - 요약 수정

**Headers:**
```
Authorization: Bearer {access_token}
Content-Type: application/json
```

**Request:**
```json
{
  "title": "수정된 제목",
  "summary_content": "수정된 요약 내용"
}
```

**Response (200 OK):**
```json
{
  "id": "660e8400-e29b-41d4-a716-446655440000",
  "title": "수정된 제목",
  "summary_content": "수정된 요약 내용",
  "updated_at": "2025-10-28T11:00:00Z"
}
```

---

#### 5.2.7 DELETE `/api/v1/summaries/{id}` - 요약 삭제

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response (204 No Content)**

---

#### 5.2.8 GET `/api/v1/summaries/{id}/status` - 처리 상태 조회

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response (200 OK):**
```json
{
  "id": "660e8400-e29b-41d4-a716-446655440000",
  "status": "processing",
  "progress_percentage": 65,
  "estimated_remaining_seconds": 5
}
```

**Status Values:**
- `pending`: 대기 중
- `processing`: 처리 중
- `completed`: 완료
- `failed`: 실패

---

### 5.3 에러 응답 형식

모든 에러 응답은 다음 형식을 따릅니다:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "요청 데이터가 유효하지 않습니다",
    "details": [
      {
        "field": "email",
        "message": "유효한 이메일 형식이 아닙니다"
      }
    ],
    "timestamp": "2025-10-28T10:00:00Z",
    "request_id": "req_123456789"
  }
}
```

**에러 코드 목록:**
- `VALIDATION_ERROR`: 입력 유효성 검증 실패
- `AUTHENTICATION_FAILED`: 인증 실패
- `AUTHORIZATION_FAILED`: 권한 없음
- `RESOURCE_NOT_FOUND`: 리소스를 찾을 수 없음
- `DUPLICATE_RESOURCE`: 중복된 리소스
- `RATE_LIMIT_EXCEEDED`: API 호출 한도 초과
- `INTERNAL_SERVER_ERROR`: 서버 내부 오류
- `SERVICE_UNAVAILABLE`: 서비스 일시 중단

### 5.4 Rate Limiting

- **인증되지 않은 요청**: 100 요청/시간
- **인증된 요청**: 1000 요청/시간
- **요약 생성**: 50 요청/시간

Rate Limit 초과 시 응답:
```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "API 호출 한도를 초과했습니다",
    "retry_after": 3600
  }
}
```

---

## 6. 에이전트 사양

### 6.1 에이전트 개요

**목적**: LangChain 기반 AI 에이전트를 통해 텍스트 요약, 태그 생성, 웹 스크래핑 등의 작업 수행

**주요 역할**:
1. 텍스트 요약 생성 (다양한 길이/언어)
2. 자동 태그 및 키워드 추출
3. 웹 페이지 콘텐츠 크롤링
4. 문서 파일 파싱 (PDF, DOCX)

### 6.2 에이전트 구조

```mermaid
graph TB
    A[Celery Task] --> B[Summary Agent]
    B --> C{Input Type?}
    C -->|Text| D[Direct Processing]
    C -->|URL| E[Web Scraper Tool]
    C -->|File| F[Document Parser Tool]
    D --> G[LLM: GPT-4]
    E --> H[Extract Content]
    F --> I[Parse Document]
    H --> G
    I --> G
    G --> J[Generate Summary]
    J --> K[Generate Tags]
    K --> L[Return Results]
```

### 6.3 프로젝트 구조

```
agent/
├── __init__.py
├── summary_agent.py        # 메인 에이전트
├── tools/
│   ├── __init__.py
│   ├── web_scraper.py      # 웹 스크래핑 도구
│   ├── document_parser.py  # 문서 파싱 도구
│   └── tag_generator.py    # 태그 생성 도구
├── prompts/
│   ├── __init__.py
│   ├── summary_prompts.py  # 요약 프롬프트
│   └── tag_prompts.py      # 태그 프롬프트
└── utils/
    ├── __init__.py
    ├── token_counter.py
    └── text_cleaner.py
```

### 6.4 에이전트 구현

#### 메인 에이전트
```python
# agent/summary_agent.py
from langchain.agents import AgentExecutor, create_openai_functions_agent
from langchain.chat_models import ChatOpenAI
from langchain.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain.tools import Tool
from typing import Dict, List
import time

class SummaryAgent:
    def __init__(self):
        self.llm = ChatOpenAI(
            model="gpt-4",
            temperature=0.3,
            max_tokens=2000
        )
        
        self.tools = [
            self._create_web_scraper_tool(),
            self._create_document_parser_tool(),
            self._create_tag_generator_tool()
        ]
        
        self.agent = self._create_agent()
    
    def summarize(
        self, 
        content: str, 
        length: str = "medium",
        language: str = "ko"
    ) -> Dict:
        """
        텍스트 요약 생성
        
        Args:
            content: 요약할 텍스트
            length: 요약 길이 (short, medium, long)
            language: 언어 (ko, en)
        
        Returns:
            {
                'summary': str,
                'token_count': int,
                'processing_time_ms': int
            }
        """
        start_time = time.time()
        
        # 프롬프트 생성
        prompt = self._create_summary_prompt(content, length, language)
        
        # LLM 호출
        response = self.llm.predict(prompt)
        
        # 토큰 카운트
        token_count = self._count_tokens(content + response)
        
        processing_time = int((time.time() - start_time) * 1000)
        
        return {
            'summary': response.strip(),
            'token_count': token_count,
            'processing_time_ms': processing_time
        }
    
    def generate_tags(
        self, 
        original_content: str, 
        summary: str,
        max_tags: int = 5
    ) -> List[str]:
        """
        자동 태그 생성
        
        Args:
            original_content: 원본 텍스트
            summary: 요약 텍스트
            max_tags: 최대 태그 개수
        
        Returns:
            List of tag names
        """
        prompt = self._create_tag_prompt(original_content, summary, max_tags)
        response = self.llm.predict(prompt)
        
        # 태그 파싱 (쉼표로 구분)
        tags = [tag.strip() for tag in response.split(',')]
        return tags[:max_tags]
    
    def scrape_and_summarize(
        self, 
        url: str, 
        length: str = "medium",
        language: str = "ko"
    ) -> Dict:
        """
        URL 크롤링 및 요약
        """
        from tools.web_scraper import WebScraperTool
        
        scraper = WebScraperTool()
        content = scraper.scrape(url)
        
        if not content:
            raise ValueError("웹 페이지에서 콘텐츠를 추출할 수 없습니다")
        
        return self.summarize(content, length, language)
    
    def parse_and_summarize(
        self, 
        file_path: str, 
        length: str = "medium",
        language: str = "ko"
    ) -> Dict:
        """
        파일 파싱 및 요약
        """
        from tools.document_parser import DocumentParserTool
        
        parser = DocumentParserTool()
        content = parser.parse(file_path)
        
        if not content:
            raise ValueError("파일에서 텍스트를 추출할 수 없습니다")
        
        return self.summarize(content, length, language)
    
    def _create_summary_prompt(
        self, 
        content: str, 
        length: str, 
        language: str
    ) -> str:
        """요약 프롬프트 생성"""
        from prompts.summary_prompts import get_summary_prompt
        return get_summary_prompt(content, length, language)
    
    def _create_tag_prompt(
        self, 
        original: str, 
        summary: str, 
        max_tags: int
    ) -> str:
        """태그 생성 프롬프트"""
        from prompts.tag_prompts import get_tag_prompt
        return get_tag_prompt(original, summary, max_tags)
    
    def _count_tokens(self, text: str) -> int:
        """토큰 카운트"""
        from utils.token_counter import count_tokens
        return count_tokens(text)
```

#### 웹 스크래퍼 도구
```python
# agent/tools/web_scraper.py
import requests
from bs4 import BeautifulSoup
from typing import Optional
import re

class WebScraperTool:
    def __init__(self):
        self.headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
        }
    
    def scrape(self, url: str) -> Optional[str]:
        """
        웹 페이지에서 메인 콘텐츠 추출
        
        Args:
            url: 크롤링할 URL
        
        Returns:
            추출된 텍스트 또는 None
        """
        try:
            response = requests.get(url, headers=self.headers, timeout=10)
            response.raise_for_status()
            
            soup = BeautifulSoup(response.content, 'html.parser')
            
            # 불필요한 태그 제거
            for tag in soup(['script', 'style', 'nav', 'footer', 'header']):
                tag.decompose()
            
            # 메인 콘텐츠 추출 시도
            main_content = (
                soup.find('article') or 
                soup.find('main') or 
                soup.find('div', class_=re.compile('content|article|post'))
            )
            
            if main_content:
                text = main_content.get_text(separator='\n', strip=True)
            else:
                text = soup.get_text(separator='\n', strip=True)
            
            # 텍스트 정제
            text = self._clean_text(text)
            
            return text if len(text) > 100 else None
            
        except Exception as e:
            print(f"Scraping error: {e}")
            return None
    
    def _clean_text(self, text: str) -> str:
        """텍스트 정제"""
        # 중복 공백 제거
        text = re.sub(r'\s+', ' ', text)
        # 중복 줄바꿈 제거
        text = re.sub(r'\n+', '\n', text)
        return text.strip()
```

#### 문서 파서 도구
```python
# agent/tools/document_parser.py
from typing import Optional
import os
from PyPDF2 import PdfReader
from docx import Document

class DocumentParserTool:
    def parse(self, file_path: str) -> Optional[str]:
        """
        문서 파일에서 텍스트 추출
        
        Args:
            file_path: 파일 경로
        
        Returns:
            추출된 텍스트 또는 None
        """
        ext = os.path.splitext(file_path)[1].lower()
        
        if ext == '.pdf':
            return self._parse_pdf(file_path)
        elif ext in ['.docx', '.doc']:
            return self._parse_docx(file_path)
        elif ext == '.txt':
            return self._parse_txt(file_path)
        else:
            raise ValueError(f"지원하지 않는 파일 형식: {ext}")
    
    def _parse_pdf(self, file_path: str) -> str:
        """PDF 파싱"""
        reader = PdfReader(file_path)
        text = ""
        for page in reader.pages:
            text += page.extract_text() + "\n"
        return text.strip()
    
    def _parse_docx(self, file_path: str) -> str:
        """DOCX 파싱"""
        doc = Document(file_path)
        text = "\n".join([para.text for para in doc.paragraphs])
        return text.strip()
    
    def _parse_txt(self, file_path: str) -> str:
        """TXT 파싱"""
        with open(file_path, 'r', encoding='utf-8') as f:
            return f.read().strip()
```

### 6.5 프롬프트 템플릿

#### 요약 프롬프트
```python
# agent/prompts/summary_prompts.py

SUMMARY_PROMPTS = {
    'ko': {
        'short': """다음 텍스트를 2-3 문장으로 간단히 요약해주세요. 핵심 내용만 포함하세요.

텍스트:
{content}

요약:""",
        'medium': """다음 텍스트를 5-7 문장으로 요약해주세요. 주요 포인트와 세부 내용을 균형있게 포함하세요.

텍스트:
{content}

요약:""",
        'long': """다음 텍스트를 상세하게 요약해주세요 (10-15 문장). 주요 논점, 근거, 결론을 모두 포함하세요.

텍스트:
{content}

요약:"""
    },
    'en': {
        'short': """Summarize the following text in 2-3 sentences. Include only the key points.

Text:
{content}

Summary:""",
        'medium': """Summarize the following text in 5-7 sentences. Balance main points with supporting details.

Text:
{content}

Summary:""",
        'long': """Provide a detailed summary of the following text (10-15 sentences). Include main arguments, evidence, and conclusions.

Text:
{content}

Summary:"""
    }
}

def get_summary_prompt(content: str, length: str, language: str) -> str:
    template = SUMMARY_PROMPTS.get(language, SUMMARY_PROMPTS['ko'])[length]
    return template.format(content=content)
```

#### 태그 프롬프트
```python
# agent/prompts/tag_prompts.py

TAG_PROMPT_TEMPLATE = {
    'ko': """다음 텍스트와 요약을 분석하여 {max_tags}개의 관련 태그를 생성해주세요.
태그는 쉼표로 구분하고, 구체적이고 검색에 유용한 키워드를 선택하세요.

원본:
{original}

요약:
{summary}

태그 (쉼표로 구분):""",
    'en': """Analyze the following text and summary to generate {max_tags} relevant tags.
Separate tags with commas and choose specific, searchable keywords.

Original:
{original}

Summary:
{summary}

Tags (comma-separated):"""
}

def get_tag_prompt(
    original: str, 
    summary: str, 
    max_tags: int, 
    language: str = 'ko'
) -> str:
    template = TAG_PROMPT_TEMPLATE.get(language, TAG_PROMPT_TEMPLATE['ko'])
    return template.format(
        original=original[:500],  # 처음 500자만
        summary=summary,
        max_tags=max_tags
    )
```

### 6.6 에이전트 통합 플로우

```mermaid
sequenceDiagram
    participant U as User
    participant API as FastAPI
    participant Q as Celery Queue
    participant A as Agent
    participant LLM as GPT-4
    participant DB as Database
    
    U->>API: POST /summaries
    API->>DB: Create Summary (status: pending)
    API->>Q: Enqueue Task
    API->>U: 202 Accepted
    
    Q->>A: Process Task
    A->>DB: Update Status (processing)
    
    alt URL Input
        A->>A: Web Scraper
    else File Input
        A->>A: Document Parser
    end
    
    A->>LLM: Generate Summary
    LLM->>A: Summary Result
    
    A->>LLM: Generate Tags
    LLM->>A: Tags Result
    
    A->>DB: Update Summary (completed)
    A->>DB: Save Tags
    
    U->>API: GET /summaries/{id}
    API->>DB: Query Summary
    DB->>API: Summary Data
    API->>U: 200 OK with Summary
```

---

## 7. 데이터 모델 (Pydantic Schemas)

### 7.1 사용자 스키마

```python
# schemas/user.py
from pydantic import BaseModel, EmailStr, Field
from typing import Optional
from datetime import datetime
from uuid import UUID

class UserBase(BaseModel):
    email: EmailStr
    username: str = Field(..., min_length=4, max_length=20, pattern=r'^[a-zA-Z0-9_]+$')
    full_name: Optional[str] = Field(None, max_length=255)

class UserCreate(UserBase):
    password: str = Field(..., min_length=8, max_length=100)

class UserUpdate(BaseModel):
    full_name: Optional[str] = None
    email: Optional[EmailStr] = None

class UserResponse(UserBase):
    id: UUID
    is_active: bool
    created_at: datetime
    
    class Config:
        from_attributes = True

class UserWithStats(UserResponse):
    total_summaries: int
    total_tags: int
    favorite_count: int
```

### 7.2 요약 스키마

```python
# schemas/summary.py
from pydantic import BaseModel, Field, HttpUrl
from typing import Optional, List
from datetime import datetime
from uuid import UUID
from enum import Enum

class InputType(str, Enum):
    TEXT = "text"
    FILE = "file"
    URL = "url"

class SummaryLength(str, Enum):
    SHORT = "short"
    MEDIUM = "medium"
    LONG = "long"

class SummaryStatus(str, Enum):
    PENDING = "pending"
    PROCESSING = "processing"
    COMPLETED = "completed"
    FAILED = "failed"

class SummaryOptions(BaseModel):
    summary_length: SummaryLength = SummaryLength.MEDIUM
    language: str = Field(default="ko", pattern=r'^[a-z]{2}$')
    auto_tag: bool = True

class SummaryCreateText(BaseModel):
    input_type: InputType = InputType.TEXT
    content: str = Field(..., min_length=100, max_length=50000)
    title: Optional[str] = Field(None, max_length=500)
    options: SummaryOptions = SummaryOptions()

class SummaryCreateURL(BaseModel):
    input_type: InputType = InputType.URL
    source_url: HttpUrl
    title: Optional[str] = Field(None, max_length=500)
    options: SummaryOptions = SummaryOptions()

class SummaryResponse(BaseModel):
    id: UUID
    user_id: UUID
    title: Optional[str]
    original_content: str
    summary_content: Optional[str]
    input_type: InputType
    source_url: Optional[str]
    language: str
    summary_length: Optional[SummaryLength]
    status: SummaryStatus
    processing_time_ms: Optional[int]
    token_count: Optional[int]
    is_favorite: bool
    tags: List['TagResponse'] = []
    created_at: datetime
    updated_at: datetime
    
    class Config:
        from_attributes = True

class SummaryListItem(BaseModel):
    id: UUID
    title: Optional[str]
    summary_content: Optional[str]
    status: SummaryStatus
    is_favorite: bool
    tags: List[str]
    created_at: datetime
    
    class Config:
        from_attributes = True

class SummaryUpdate(BaseModel):
    title: Optional[str] = None
    summary_content: Optional[str] = None

class SummaryStatusResponse(BaseModel):
    id: UUID
    status: SummaryStatus
    progress_percentage: Optional[int] = None
    estimated_remaining_seconds: Optional[int] = None
```

### 7.3 태그 스키마

```python
# schemas/tag.py
from pydantic import BaseModel, Field
from typing import Optional
from datetime import datetime
from uuid import UUID

class TagBase(BaseModel):
    name: str = Field(..., min_length=1, max_length=100)
    color: Optional[str] = Field(None, pattern=r'^#[0-9A-Fa-f]{6}$')

class TagCreate(TagBase):
    pass

class TagResponse(TagBase):
    id: UUID
    created_at: datetime
    
    class Config:
        from_attributes = True

class TagWithCount(TagResponse):
    usage_count: int

class SummaryTagUpdate(BaseModel):
    tag_ids: List[UUID]
```

### 7.4 페이지네이션 스키마

```python
# schemas/pagination.py
from pydantic import BaseModel, Field
from typing import Generic, TypeVar, List

T = TypeVar('T')

class PaginationParams(BaseModel):
    page: int = Field(default=1, ge=1)
    page_size: int = Field(default=20, ge=1, le=100)

class PaginatedResponse(BaseModel, Generic[T]):
    items: List[T]
    total: int
    page: int
    page_size: int
    total_pages: int
    
    @classmethod
    def create(cls, items: List[T], total: int, page: int, page_size: int):
        return cls(
            items=items,
            total=total,
            page=page,
            page_size=page_size,
            total_pages=(total + page_size - 1) // page_size
        )
```

---

## 8. 비기능적 요구사항

### 8.1 성능 요구사항

| 메트릭 | 목표 | 측정 방법 |
|--------|------|-----------|
| API 응답 시간 (GET) | < 200ms (p95) | Prometheus + Grafana |
| API 응답 시간 (POST) | < 500ms (p95) | Prometheus + Grafana |
| 요약 생성 시간 | < 30초 (중간 길이) | Application Logs |
| 데이터베이스 쿼리 | < 100ms (p95) | PostgreSQL Logs |
| 페이지 로드 시간 | < 2초 | Lighthouse |
| 동시 사용자 | 1000명 | Load Testing (Locust) |

### 8.2 보안 요구사항

#### 8.2.1 인증 및 인가
- JWT 기반 인증 (Access Token + Refresh Token)
- Access Token 만료: 30분
- Refresh Token 만료: 7일
- 비밀번호 해싱: bcrypt (cost factor 12)

#### 8.2.2 데이터 보호
- HTTPS 필수 (TLS 1.3)
- 데이터베이스 연결 암호화
- 민감 정보 (API 키, 비밀번호) 환경 변수로 관리
- S3 버킷: Private, Pre-signed URLs 사용

#### 8.2.3 입력 검증
- 모든 사용자 입력 검증 (Pydantic)
- SQL Injection 방지 (ORM 사용)
- XSS 방지 (React 자동 이스케이핑)
- CSRF 방지 (SameSite Cookie)

#### 8.2.4 Rate Limiting
- IP 기반 제한
- 사용자 기반 제한
- Redis를 이용한 분산 Rate Limiting

### 8.3 확장성

#### 8.3.1 수평 확장
- **Frontend**: CDN + Static Hosting (Vercel, Cloudflare Pages)
- **Backend**: Auto Scaling Group (AWS ECS/EKS)
- **Database**: Read Replicas (PostgreSQL Streaming Replication)
- **Cache**: Redis Cluster
- **Worker**: Celery Auto Scaling

#### 8.3.2 데이터베이스 최적화
- 인덱스 전략
  - B-tree: ID, 외래 키
  - GIN: 전문 검색 (full-text search)
  - Partial: 조건부 인덱스
- 파티셔닝
  - Summaries 테이블: 월별 파티션
- 연결 풀링
  - Min: 5, Max: 20 connections

### 8.4 가용성 (Availability)

**목표 SLA**: 99.9% (연간 다운타임 < 8.76시간)

#### 전략:
1. **Multi-AZ 배포** (AWS)
   - RDS: Multi-AZ
   - ElastiCache: Multi-AZ
   - Application Servers: 최소 2개 AZ

2. **Health Checks**
   - ELB Health Check: 30초 간격
   - Application Health Endpoint: `/health`

3. **백업**
   - 데이터베이스: 일일 자동 백업, 7일 보관
   - S3: Cross-Region Replication

4. **모니터링 & 알림**
   - Uptime monitoring (UptimeRobot)
   - Error rate > 5%: 즉시 알림
   - Response time > 1s: 경고

### 8.5 모니터링 및 로깅

#### 8.5.1 애플리케이션 모니터링
- **도구**: Prometheus + Grafana
- **메트릭**:
  - Request rate (req/s)
  - Error rate (%)
  - Response time (p50, p95, p99)
  - Active users
  - Queue length (Celery)

#### 8.5.2 로깅
- **도구**: ELK Stack (Elasticsearch, Logstash, Kibana)
- **로그 레벨**:
  - ERROR: 즉시 알림
  - WARN: 일일 리포트
  - INFO: 주요 이벤트
  - DEBUG: 개발 환경만
  
- **로그 포맷** (JSON):
```json
{
  "timestamp": "2025-10-28T10:00:00Z",
  "level": "INFO",
  "service": "api",
  "request_id": "req_123",
  "user_id": "user_456",
  "endpoint": "/api/v1/summaries",
  "method": "POST",
  "status_code": 201,
  "duration_ms": 245,
  "message": "Summary created successfully"
}
```

#### 8.5.3 추적 (Tracing)
- **도구**: Jaeger / OpenTelemetry
- **추적 범위**:
  - API 요청 → 서비스 → 데이터베이스
  - Celery 작업 → Agent → LLM

### 8.6 테스트 요구사항

#### 8.6.1 단위 테스트
- **커버리지**: 최소 80%
- **도구**: pytest (Backend), Jest (Frontend)
- **대상**:
  - 모든 서비스 레이어 함수
  - 유틸리티 함수
  - React 컴포넌트

#### 8.6.2 통합 테스트
- **도구**: pytest + TestClient (FastAPI)
- **대상**:
  - API 엔드포인트
  - 데이터베이스 트랜잭션
  - Celery 작업

#### 8.6.3 E2E 테스트
- **도구**: Playwright / Cypress
- **시나리오**:
  - 사용자 회원가입 → 로그인
  - 텍스트 요약 생성 → 결과 확인
  - 히스토리 조회 및 검색

#### 8.6.4 부하 테스트
- **도구**: Locust
- **시나리오**:
  - 1000 동시 사용자
  - 요약 생성 API: 100 req/s
  - 목표 응답 시간 유지

### 8.7 배포 및 운영

#### 8.7.1 CI/CD 파이프라인
```mermaid
graph LR
    A[Git Push] --> B[GitHub Actions]
    B --> C[Run Tests]
    C --> D{Tests Pass?}
    D -->|Yes| E[Build Docker Images]
    D -->|No| F[Notify Developer]
    E --> G[Push to ECR]
    G --> H[Deploy to Staging]
    H --> I[Integration Tests]
    I --> J{Tests Pass?}
    J -->|Yes| K[Deploy to Production]
    J -->|No| F
    K --> L[Health Check]
    L --> M{Healthy?}
    M -->|Yes| N[Complete]
    M -->|No| O[Rollback]
```

#### 8.7.2 환경 구성
- **Development**: 로컬 Docker Compose
- **Staging**: AWS ECS (1 instance)
- **Production**: AWS ECS (Auto Scaling, 2-10 instances)

#### 8.7.3 배포 전략
- **Zero-downtime deployment**: Blue-Green Deployment
- **Rollback 시간**: < 5분
- **Database Migration**: Alembic, 배포 전 자동 실행

---

## 9. 기술적 제약사항 및 가정

### 9.1 제약사항

1. **LLM API**
   - OpenAI API Rate Limits 준수
   - 토큰 제한: 입력 + 출력 < 4096 tokens (GPT-4)
   - 비용 고려: 캐싱 전략 필수

2. **파일 업로드**
   - 최대 파일 크기: 10MB
   - 지원 형식: PDF, DOCX, TXT
   - 바이러스 스캔 필요 (ClamAV)

3. **웹 스크래핑**
   - Robots.txt 준수
   - Rate Limiting (동일 도메인 5초 간격)
   - JavaScript 렌더링 제한적 지원

4. **데이터 보관**
   - 요약 데이터: 무제한 보관
   - 파일: 90일 후 자동 삭제 (옵션)

### 9.2 가정

1. **사용자**
   - 대부분 한국어/영어 사용자
   - 평균 문서 길이: 1000-5000 단어
   - 일일 활성 사용자: 100-1000명 (초기)

2. **인프라**
   - AWS 사용
   - 초기 트래픽: < 100 req/s
   - 데이터베이스 크기: < 100GB (1년)

3. **비용**
   - 월 예산: $500-1000
   - OpenAI API: 요약당 평균 $0.05

---

## 10. 구현 우선순위 및 로드맵

### Phase 1: MVP (4-6주)
- ✅ 사용자 인증 (회원가입, 로그인)
- ✅ 텍스트 입력 요약 기능
- ✅ 기본 UI (대시보드, 요약 생성, 히스토리)
- ✅ 기본 태그 생성
- ✅ 데이터베이스 설정
- ✅ 기본 배포 (Staging)

### Phase 2: 핵심 기능 (4주)
- ✅ URL 크롤링 및 요약
- ✅ 파일 업로드 및 파싱
- ✅ 요약 길이 조절
- ✅ 태그 편집 기능
- ✅ 즐겨찾기
- ✅ 검색 및 필터링

### Phase 3: 최적화 (2-3주)
- ✅ 캐싱 구현 (Redis)
- ✅ Rate Limiting
- ✅ 성능 최적화
- ✅ 에러 핸들링 개선
- ✅ Production 배포

### Phase 4: 고급 기능 (4주)
- ⬜ 다국어 지원 확대
- ⬜ 요약 스타일 선택 (공식적/캐주얼)
- ⬜ PDF/TXT 다운로드
- ⬜ 공유 기능
- ⬜ 사용자 피드백 시스템

### Phase 5: 스케일링 (2주)
- ⬜ 모니터링 대시보드
- ⬜ Auto Scaling 설정
- ⬜ Multi-Region 배포 (선택)
- ⬜ 성능 테스트 및 최적화

---

## 11. 성공 지표 (KPIs)

### 11.1 비즈니스 지표
- **활성 사용자 수**: 월간 활성 사용자 (MAU)
- **요약 생성 수**: 일일 요약 생성 건수
- **사용자 유지율**: 30일 유지율 > 40%
- **평균 세션 시간**: > 5분

### 11.2 기술 지표
- **API 가용성**: > 99.9%
- **평균 응답 시간**: < 500ms
- **에러율**: < 1%
- **요약 성공률**: > 95%

### 11.3 품질 지표
- **사용자 만족도**: 평균 별점 > 4.0/5.0
- **요약 품질 피드백**: 긍정 피드백 > 80%

---

## 12. 리스크 및 완화 전략

| 리스크 | 영향도 | 확률 | 완화 전략 |
|--------|--------|------|-----------|
| OpenAI API 장애 | 높음 | 낮음 | Fallback LLM 준비 (Anthropic Claude) |
| 비용 초과 | 중간 | 중간 | 월별 예산 알림, 캐싱 강화 |
| 스크래핑 차단 | 중간 | 중간 | User-Agent 로테이션, Proxy 사용 |
| 데이터베이스 성능 저하 | 높음 | 낮음 | 인덱스 최적화, Read Replica |
| 보안 침해 | 높음 | 낮음 | 정기 보안 감사, OWASP Top 10 준수 |

---

## 13. 부록

### 13.1 환경 변수 예시

```bash
# .env.example

# Application
APP_NAME=auto-summary-service
APP_ENV=production
DEBUG=false
SECRET_KEY=your-secret-key-change-this

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/summary_db
DATABASE_POOL_SIZE=20

# Redis
REDIS_URL=redis://localhost:6379/0

# Celery
CELERY_BROKER_URL=redis://localhost:6379/0
CELERY_RESULT_BACKEND=redis://localhost:6379/0

# OpenAI
OPENAI_API_KEY=sk-xxxxxxxxxxxxx
OPENAI_MODEL=gpt-4
OPENAI_MAX_TOKENS=2000

# AWS
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
AWS_REGION=ap-northeast-2
S3_BUCKET_NAME=summary-files

# JWT
JWT_SECRET_KEY=your-jwt-secret
JWT_ALGORITHM=HS256
JWT_ACCESS_TOKEN_EXPIRE_MINUTES=30
JWT_REFRESH_TOKEN_EXPIRE_DAYS=7

# CORS
CORS_ORIGINS=http://localhost:3000,https://yourdomain.com

# Rate Limiting
RATE_LIMIT_PER_HOUR=1000
RATE_LIMIT_SUMMARY_PER_HOUR=50
```

### 13.2 Docker Compose 예시

```yaml
version: '3.8'

services:
  # PostgreSQL
  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: summary_db
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  # Redis
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

  # Backend API
  api:
    build: ./backend
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
    ports:
      - "8000:8000"
    environment:
      DATABASE_URL: postgresql://postgres:postgres@db:5432/summary_db
      REDIS_URL: redis://redis:6379/0
    depends_on:
      - db
      - redis
    volumes:
      - ./backend:/app

  # Celery Worker
  worker:
    build: ./backend
    command: celery -A app.tasks.celery_app worker --loglevel=info
    environment:
      DATABASE_URL: postgresql://postgres:postgres@db:5432/summary_db
      REDIS_URL: redis://redis:6379/0
    depends_on:
      - db
      - redis
    volumes:
      - ./backend:/app

  # Frontend
  frontend:
    build: ./frontend
    command: npm start
    ports:
      - "3000:3000"
    environment:
      REACT_APP_API_URL: http://localhost:8000/api/v1
    volumes:
      - ./frontend:/app
      - /app/node_modules

volumes:
  postgres_data:
  redis_data:
```

### 13.3 참고 자료

- **FastAPI 공식 문서**: https://fastapi.tiangolo.com/
- **LangChain 문서**: https://python.langchain.com/
- **React 공식 문서**: https://react.dev/
- **PostgreSQL 문서**: https://www.postgresql.org/docs/
- **OpenAI API 문서**: https://platform.openai.com/docs/

---

## 문서 변경 이력

| 버전 | 날짜 | 변경 내용 | 작성자 |
|------|------|-----------|--------|
| 1.0.0 | 2025-10-28 | 초안 작성 | AI Agent |

---

**문서 승인**
- [ ] 프로덕트 오너
- [ ] 기술 리드
- [ ] 시니어 개발자

**다음 리뷰 예정일**: 2025-11-15
