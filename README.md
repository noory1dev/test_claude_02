# 백엔드 개발자 포트폴리오 웹사이트

기획서의 모든 요구사항을 반영한 **전문적이고 신뢰감 있는** 백엔드 개발자 포트폴리오 웹사이트입니다.

## 📋 프로젝트 구조

```
test_claude_02/
├── index.html          # 메인 HTML 파일
├── styles.css          # 스타일시트 (다크 모드 + 터키블루 포인트 컬러)
├── script.js           # 인터랙티브 기능
├── README.md           # 이 파일
└── doc/
    ├── START_HERE.md	            # 🚀 시작 가이드 (먼저 읽으세요!)
    ├── PROJECT_GUIDE.md	        # 프로젝트 작성 가이드 + 상세 템플릿
    ├── DEPLOYMENT_GUIDE.md	        # 배포 및 최적화 방법 (GitHub Pages, Netlify 등)
    ├── 개발자 포트폴리오 기획서_260622.docx    # 원본 기획서
    └── 개발자 포트폴리오 기획서_260622.pdf     # PDF 버전
```

## 🎨 주요 기능

### 1. **Hero Section (첫 화면)**
- 백엔드 개발자로서의 지향점 표시
- 시스템 아키텍처 플로우 시각화
- CTA 버튼 (프로젝트 보기, 연락하기)

### 2. **About Me (소개)**
- 커리어 요약 및 기술적 관심사
- 주요 성과 통계 카드
  - 5년 이상 경력
  - 50+ 프로젝트 완료
  - 100M+ 일일 요청 처리
  - 99.9% 가동률

### 3. **Tech Stack (기술 스택)**
6가지 카테고리로 명확히 분류:
- **Backend Languages**: Java, Python, Node.js, Go
- **Frameworks**: Spring Boot, FastAPI, Express, Django
- **Database & Cache**: PostgreSQL, MySQL, MongoDB, Redis
- **DevOps & Infrastructure**: AWS, Docker, Kubernetes, CI/CD
- **Tools & Monitoring**: Git, Jira, Grafana, ELK Stack
- **Message Queue & Stream**: Kafka, RabbitMQ, Apache Storm

### 4. **Projects (포트폴리오 핵심)**
각 프로젝트마다 다음 정보 포함:
- 프로젝트 개요
- 시스템 아키텍처 다이어그램
- **정량화된 성과 (Metrics)**
  - 쿼리 성능 개선 (예: 1,200ms → 80ms)
  - 응답 시간 개선 (예: 45% 단축)
  - 처리량 증가 (예: 3배 증가)
- **기술적 도전과 해결 (Troubleshooting)**
  - N+1 쿼리 문제 해결
  - 데드락 개선
  - 성능 병목 제거

### 5. **Contact (연락처)**
- GitHub 링크
- LinkedIn 링크
- 기술 블로그
- 이메일 연락

## 🎯 커스터마이제이션 가이드

### 1. 개인 정보 수정

**index.html**에서 다음 부분을 수정하세요:

```html
<!-- Hero 섹션 -->
<h2 class="hero-subtitle">Backend & DB Developer</h2>
<!-- 이 부분을 사용자명으로 변경 -->

<!-- About 섹션의 통계 수정 -->
<div class="stat-number">5+</div>  <!-- 경력 연수 -->
<div class="stat-number">50+</div> <!-- 프로젝트 수 -->
<div class="stat-number">100M+</div> <!-- 일일 요청 -->
<div class="stat-number">99.9%</div> <!-- 가동률 -->

<!-- Contact 섹션의 링크 수정 -->
<a href="https://github.com/your-username" target="_blank">GitHub</a>
<a href="https://linkedin.com/in/your-profile" target="_blank">LinkedIn</a>
<a href="https://your-blog.com" target="_blank">Tech Blog</a>
<a href="mailto:your-email@example.com">Email</a>
```

### 2. Tech Stack 수정

**index.html**의 Tech Stack 섹션에서 배지 추가/제거:

```html
<div class="tech-category">
    <h3><i class="fas fa-code"></i> Backend Languages</h3>
    <div class="tech-list">
        <span class="tech-badge">Java</span>
        <span class="tech-badge">Python</span>
        <!-- 더하기: 자신의 기술 스택 추가 -->
    </div>
</div>
```

### 3. 프로젝트 추가/수정

프로젝트 카드를 복사하여 새 프로젝트 추가:

```html
<!-- Project Card Template -->
<div class="project-card">
    <div class="project-header">
        <h3>프로젝트명</h3>
        <span class="project-tag">카테고리</span>
    </div>
    <p class="project-description">설명</p>
    <div class="project-details">
        <div class="detail-item">
            <span class="label">기술 스택:</span>
            <span class="value">기술들</span>
        </div>
        <div class="detail-item">
            <span class="label">역할:</span>
            <span class="value">역할</span>
        </div>
    </div>
    <button class="btn-expand" onclick="toggleProjectDetail(this)">상세 보기</button>
    <div class="project-detail hidden">
        <!-- 아키텍처, 성과, 도전과 해결 정보 -->
    </div>
</div>
```

### 4. 색상 커스터마이제이션

**styles.css**의 :root 섹션에서 색상 변경:

```css
:root {
    --color-bg: #0f1419;          /* 배경색 */
    --color-bg-light: #1a2332;    /* 밝은 배경색 */
    --color-text: #e0e0e0;        /* 텍스트 색 */
    --color-text-light: #a0a0a0;  /* 밝은 텍스트 색 */
    --color-primary: #00d4ff;     /* 주요 포인트 색 (터키블루) */
    --color-secondary: #1a8a96;   /* 보조 색상 */
    --color-accent: #ff6b9d;      /* 강조 색상 */
    --color-border: #2d3748;      /* 테두리 색 */
}
```

## 📱 반응형 디자인

- **데스크톱** (1200px 이상): 최적화된 레이아웃
- **태블릿** (768px ~ 1199px): 조정된 그리드 레이아웃
- **모바일** (480px ~ 767px): 스택 레이아웃
- **초소형** (480px 이하): 최소화된 레이아웃

## 🚀 시작하기

### 1. 로컬에서 실행

간단하게 파일을 열어서 보기:

```bash
# 방법 1: 직접 HTML 파일 열기
start index.html

# 방법 2: 로컬 서버로 실행 (Python)
python -m http.server 8000

# 방법 3: Live Server 사용 (VS Code 확장)
# VS Code에서 index.html 우클릭 → "Open with Live Server"
```

### 2. 호스팅

다음 플랫폼에서 무료로 호스팅 가능:

- **GitHub Pages**: GitHub에 업로드하고 Settings에서 활성화
- **Netlify**: github.com/netlify 연동 또는 드래그앤드롭
- **Vercel**: vercel.com에 업로드
- **Firebase Hosting**: firebase.google.com에서 배포

## 📚 콘텐츠 추천사항

### 프로젝트 상세 정보 작성 시 포함해야 할 내용

1. **시스템 아키텍처**
   - 유저 → 로드밸런서 → API 서버 → 캐시 → DB 흐름

2. **데이터베이스 모델링**
   - ERD (Entity Relationship Diagram)
   - 핵심 테이블 구조 설명

3. **정량화된 성과 (꼭 포함!)**
   - 쿼리 성능 개선 비율
   - 응답 시간 개선
   - 처리량/동시 접속자 수 증가
   - 가동률/안정성

4. **기술적 도전과 해결**
   - 발생했던 문제
   - 원인 분석
   - 해결 방법
   - 결과/성과

### 예시: 성과 지표 작성

✅ 좋은 예시:
- "복잡한 쿼리 최적화로 1,200ms → 80ms (93% 개선)"
- "Redis 캐싱 도입으로 API 응답 시간 45% 단축"
- "데이터베이스 인덱싱 전략으로 동시 접속자 처리량 3배 증가"

❌ 피해야 할 예시:
- "쿼리를 최적화했습니다" (정량화 없음)
- "성능이 좋아졌습니다" (구체성 없음)

## 🔗 추가 리소스

### 기술 블로그 추천
- **Velog**: velog.io
- **Tistory**: tistory.com
- **Medium**: medium.com
- **Dev.to**: dev.to

### API 문서화
- **Swagger**: swagger.io
- **Postman**: postman.com
- **README.md**: GitHub 리포지토리에 상세 문서 작성

## 🎓 SEO 최적화 팁

```html
<!-- HTML의 <head>에 추가 (선택사항) -->
<meta name="description" content="백엔드/DB 개발자 포트폴리오 - 신뢰성과 확장성">
<meta name="keywords" content="백엔드, 개발자, 포트폴리오, 데이터베이스, 아키텍처">
<meta name="author" content="Your Name">
```

## 📧 피드백 및 개선

이 포트폴리오를 개선하기 위해:

1. **프로젝트에 GitHub 링크 추가**
   - 코드 컨벤션이 지켜진 깔끔한 코드
   - 상세한 README.md (아키텍처, 실행 방법, 테스트)

2. **Swagger/API 명세서 연동**
   - API 문서로 직접 이동하는 링크 제공

3. **기술 블로그 운영**
   - DB 최적화 관련 글
   - 아키텍처 설계 경험
   - 베스트 글 3-4개 카드로 표시

## ✨ 디자인 특징

- **다크 모드**: 개발자 친화적인 어두운 테마
- **터키블루 포인트**: 신뢰감과 전문성 표현
- **미니멀한 구성**: 복잡하지 않으면서도 세련된 느낌
- **반응형 디자인**: 모든 기기에서 완벽하게 표시
- **부드러운 애니메이션**: 상호작용 시 자연스러운 효과

---

**주의**: 포트폴리오는 설정 후 정기적으로 업데이트하세요. 최신 프로젝트와 기술을 반영하는 것이 매우 중요합니다!
