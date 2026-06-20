# 포트폴리오 배포 및 최적화 가이드

## 🌐 배포 방법

### 방법 1: GitHub Pages (완전 무료, 권장)

1. **GitHub 계정 생성**
   - https://github.com/join에서 가입

2. **새 저장소 생성**
   - 저장소명: `username.github.io`
   - 예: `john-kim.github.io`
   - 공개(Public) 설정

3. **코드 업로드**
   ```bash
   git init
   git add .
   git commit -m "Initial portfolio commit"
   git branch -M main
   git remote add origin https://github.com/your-username/your-username.github.io.git
   git push -u origin main
   ```

4. **확인**
   - 브라우저에서 `https://your-username.github.io` 접속

### 방법 2: Netlify (무료, 자동 빌드)

1. **Netlify 계정 생성**
   - https://netlify.com에서 GitHub로 가입

2. **새 사이트 연결**
   - "New site from Git" 클릭
   - GitHub 저장소 선택
   - Build 설정 (필요 없으면 스킵)
   - Deploy!

3. **커스텀 도메인 설정** (선택)
   - Site settings → Domain management
   - 도메인 추가

### 방법 3: Vercel

1. **Vercel 계정 생성**
   - https://vercel.com에서 GitHub로 가입

2. **프로젝트 import**
   - GitHub 저장소 선택
   - Deploy 클릭

### 방법 4: 개인 서버 (고급)

Apache 또는 Nginx 설정:

```nginx
# /etc/nginx/sites-available/portfolio.conf
server {
    listen 80;
    server_name your-domain.com www.your-domain.com;
    
    root /var/www/portfolio;
    index index.html index.htm;
    
    # 모든 요청을 index.html로 라우팅
    location / {
        try_files $uri $uri/ /index.html;
    }
    
    # 캐싱 설정 (이미지, CSS, JS)
    location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
    
    # GZIP 압축
    gzip on;
    gzip_types text/plain text/css text/javascript application/javascript;
    gzip_min_length 1000;
}
```

---

## ⚡ 성능 최적화

### 1. CSS/JS 최적화

**Minification (축소)**

```bash
# Google Closure Compiler 사용
java -jar closure-compiler.jar --js styles.css > styles.min.css
java -jar closure-compiler.jar --js script.js > script.min.js
```

또는 온라인 도구:
- CSS Minifier: cssminifier.com
- JS Minifier: jsminifier.com

**index.html에서 사용:**
```html
<link rel="stylesheet" href="styles.min.css">
<script src="script.min.js"></script>
```

### 2. 이미지 최적화

```bash
# ImageMagick 사용 (설치: brew install imagemagick)
convert image.png -quality 85 -resize 1920x1080 image-optimized.jpg

# 또는 온라인 도구
# TinyPNG: tinypng.com
# Compressor.io: compressor.io
```

### 3. 캐싱 전략

```html
<!-- HTML에 버전 추가 -->
<link rel="stylesheet" href="styles.css?v=1.0.0">
<script src="script.js?v=1.0.0"></script>
```

### 4. 컨텐츠 전송 네트워크 (CDN)

Font Awesome 아이콘을 CDN에서 로드:
```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
```

---

## 📈 SEO 최적화

### 1. Meta 태그 추가

**index.html의 <head>에 추가:**

```html
<!-- 기본 SEO -->
<title>Backend Developer Portfolio | 신뢰성과 확장성</title>
<meta name="description" content="5년 경력의 백엔드/DB 개발자 포트폴리오. 시스템 아키텍처, 데이터베이스 최적화, 대규모 트래픽 처리 경험.">
<meta name="keywords" content="백엔드개발자, 포트폴리오, Java, Python, Spring Boot, PostgreSQL, 데이터베이스">
<meta name="author" content="Your Name">

<!-- Open Graph (소셜 미디어 공유) -->
<meta property="og:title" content="Backend Developer Portfolio">
<meta property="og:description" content="5년 경력의 백엔드/DB 개발자 포트폴리오">
<meta property="og:type" content="website">
<meta property="og:url" content="https://your-domain.com">
<meta property="og:image" content="https://your-domain.com/og-image.jpg">

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="Backend Developer Portfolio">
<meta name="twitter:description" content="5년 경력의 백엔드/DB 개발자 포트폴리오">

<!-- Viewport -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- Canonical URL -->
<link rel="canonical" href="https://your-domain.com">

<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_ID"></script>
<script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'GA_ID');
</script>
```

### 2. robots.txt 생성

**workspace 루트에 robots.txt 생성:**

```
User-agent: *
Allow: /

Sitemap: https://your-domain.com/sitemap.xml
```

### 3. sitemap.xml 생성

**workspace 루트에 sitemap.xml 생성:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    <url>
        <loc>https://your-domain.com/</loc>
        <lastmod>2024-06-21</lastmod>
        <changefreq>weekly</changefreq>
        <priority>1.0</priority>
    </url>
    <url>
        <loc>https://your-domain.com/#about</loc>
        <lastmod>2024-06-21</lastmod>
        <changefreq>monthly</changefreq>
        <priority>0.8</priority>
    </url>
    <url>
        <loc>https://your-domain.com/#projects</loc>
        <lastmod>2024-06-21</lastmod>
        <changefreq>weekly</changefreq>
        <priority>0.9</priority>
    </url>
</urlset>
```

---

## 🔒 보안 설정

### 1. HTTPS 활성화

GitHub Pages와 Netlify는 자동으로 HTTPS 제공.

커스텀 도메인인 경우:
- **Let's Encrypt**: 무료 SSL 인증서
- **Cloudflare**: 무료 SSL + DDoS 보호

### 2. Security Headers 추가

**Netlify의 경우, netlify.toml 생성:**

```toml
[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-Content-Type-Options = "nosniff"
    X-XSS-Protection = "1; mode=block"
    Referrer-Policy = "strict-origin-when-cross-origin"
    Permissions-Policy = "geolocation=(), microphone=(), camera=()"
```

---

## 📊 분석 및 모니터링

### Google Analytics 설정

1. **Google Analytics 계정 생성**
   - https://analytics.google.com에서 계정 생성

2. **추적 코드 추가**
   - 위의 SEO 섹션 참조

3. **주요 지표 확인**
   - 방문자 수
   - 방문 시간
   - 이탈률
   - 트래픽 소스

### 검색 엔진 등록

1. **Google Search Console 등록**
   - https://search.google.com/search-console
   - sitemap.xml 제출
   - 색인 요청

2. **Bing Webmaster Tools 등록**
   - https://www.bing.com/webmasters
   - 유사한 방식으로 등록

---

## 🎯 마케팅 팁

### 1. GitHub 링크 관리

좋은 GitHub 프로필을 위해:

```markdown
# 프로젝트 이름

한 줄 설명: 이 프로젝트가 어떤 문제를 해결하는가?

## 📋 목차
- [기능](#기능)
- [설치](#설치)
- [사용법](#사용법)
- [아키텍처](#아키텍처)

## 🎯 기능
- 기능 1
- 기능 2
- 기능 3

## 🏗️ 아키텍처
[아키텍처 다이어그램]

## 📊 성과
- 성과 1: 수치
- 성과 2: 수치

## 📝 기술 스택
- Backend: Java Spring Boot
- Database: PostgreSQL
- DevOps: Docker, Kubernetes

## 🚀 시작하기
```bash
git clone ...
cd project
java -jar application.jar
```

## 📖 API 문서
[Swagger 링크]

## 👤 작성자
Your Name - [GitHub](https://github.com/username) | [LinkedIn](https://linkedin.com/in/username)
```

### 2. 기술 블로그 운영

추천 글 주제:
- "N+1 쿼리 문제 해결하기"
- "Redis 캐싱으로 API 성능 45% 개선하기"
- "마이크로서비스 아키텍처 설계"
- "데이터베이스 인덱싱 전략"
- "분산 트랜잭션 Saga 패턴"

각 글에서:
1. 문제 정의
2. 원인 분석
3. 해결책 제시
4. 코드 예시
5. 성과/결과

### 3. LinkedIn 최적화

프로필 작성:
- 상세한 경력 기술
- 주요 프로젝트 하이라이트
- 기술 스택 명시
- 성과 수치 포함

```
Principal Backend Engineer | System Architecture Specialist

✅ Expertise:
- Microservices Architecture
- Database Optimization & Tuning
- High-Traffic System Design (100M+ daily requests)
- Cloud Infrastructure (AWS, Kubernetes)

🎯 Key Achievements:
- Reduced API response time by 93% through query optimization
- Designed microservices handling 1M+ events/second
- Achieved 99.99% system uptime
```

---

## 📅 정기 업데이트 계획

### 월별
- [ ] 새 기술 스택 추가
- [ ] 프로젝트 성과 업데이트
- [ ] 기술 블로그 최소 1-2편 작성

### 분기별 (3개월)
- [ ] 포트폴리오 디자인 점검
- [ ] SEO 성과 분석
- [ ] 구글 애널리틱스 검토
- [ ] 신규 프로젝트 추가 (최소 1개)

### 연간
- [ ] 포트폴리오 대대적 리뉴얼
- [ ] 새로운 기술 트렌드 반영
- [ ] 성과 수치 종합 정리
- [ ] 링크드인/블로그 최신화

---

## ✅ 배포 전 체크리스트

배포하기 전에 확인하세요:

- [ ] 모든 링크가 정상 작동하는가?
- [ ] 모바일에서 제대로 보이는가?
- [ ] 모든 이미지가 로드되는가?
- [ ] 기술 스택이 최신인가?
- [ ] 개인정보는 제거했는가? (비밀번호, 토큰 등)
- [ ] 오타가 없는가?
- [ ] 성과 수치가 정확한가?
- [ ] Lighthouse 점수 확인했는가? (90점 이상 목표)

**Lighthouse 확인:**
```
Chrome DevTools → Lighthouse → Generate report
```

---

## 📞 배포 후 지원

배포 후 도움이 필요하면:

1. **Netlify Support**: support.netlify.com
2. **GitHub Support**: support.github.com
3. **Vercel Support**: vercel.com/support
4. **Google Search Console Help**: support.google.com/webmasters

포트폴리오는 **살아있는 문서**입니다. 정기적으로 업데이트하고 개선하세요!
