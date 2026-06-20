# 프로젝트 작성 가이드 및 템플릿

백엔드 개발자 포트폴리오에 프로젝트를 효과적으로 표현하는 방법을 설명합니다.

## 프로젝트 작성 체크리스트

### ✅ 필수 포함 요소

- [ ] 프로젝트 명
- [ ] 프로젝트 카테고리/태그
- [ ] 한 줄 설명 (20-30글자)
- [ ] 사용 기술 스택
- [ ] 담당 역할
- [ ] 시스템 아키텍처 다이어그램
- [ ] 정량화된 성과 (반드시!)
- [ ] 기술적 도전과 해결책

---

## 프로젝트 예시 작성법

### 예시 1: E-Commerce 주문 처리 시스템

```
프로젝트명: E-Commerce 주문 처리 시스템
카테고리: Microservices
한 줄 설명: 대규모 전자상거래 플랫폼의 주문 처리 백엔드 시스템 설계 및 구현

기술 스택:
- Backend: Java Spring Boot, Python FastAPI
- Database: PostgreSQL (주 DB), Redis (캐시)
- Message Queue: Apache Kafka
- DevOps: Docker, Kubernetes, AWS

역할:
- Backend 아키텍처 설계
- 데이터베이스 성능 최적화
- 마이크로서비스 간 통신 구현

시스템 아키텍처:
┌─────────────┐
│ Client Apps │
└──────┬──────┘
       │
┌──────▼──────────────┐
│  API Gateway (Kong) │
└──────┬──────────────┘
       │
┌──────▼────────────────────────────┐
│ Order Service                      │
│ Payment Service                    │
│ Inventory Service                  │
└──────┬────────────────────────────┘
       │
   ┌───┴───┐
   │       │
┌──▼──┐ ┌─▼────────┐
│Cache│ │PostgreSQL│
│Redis│ │(RW)      │
└─────┘ └──────────┘
   │
┌──▼─────────┐
│Kafka Stream│
│Event Queue │
└────────────┘

정량화된 성과:
1. 쿼리 성능 개선: 1,200ms → 80ms (93% 개선)
   - 원인: N+1 쿼리 문제
   - 해결: JPA Fetch Join, Query DSL 최적화
   
2. API 응답 시간: 45% 단축
   - 원인: 반복되는 DB 조회
   - 해결: Redis 캐싱 도입
   
3. 동시 접속자 처리량: 3배 증가
   - 원인: 인덱싱 부재
   - 해결: 전략적 인덱싱 (복합 인덱스, 부분 인덱스)
   
4. 결제 처리 대기시간: 98% 감소
   - 원인: 동기식 처리로 인한 병목
   - 해결: Kafka를 통한 비동기 처리

기술적 도전과 해결:

도전 1: N+1 쿼리 문제
┌─────────────────────────┐
│ 문제 상황                 │
├─────────────────────────┤
│ 주문 목록 조회 시        │
│ 1) 전체 주문 쿼리       │
│ 2) 각 주문마다 상품 쿼리│
│ = N개의 추가 쿼리 발생  │
│ 결과: 1초+ 소요         │
└─────────────────────────┘

원인 분석:
- 상품 정보를 Lazy Loading으로 설정
- 응답 직렬화 시점에 추가 쿼리 발생

해결책:
```java
// Before: N+1 쿼리 발생
List<Order> orders = orderRepository.findAll();
// 각 주문의 상품 정보 접근 시점에 N개의 쿼리 발생

// After: Fetch Join으로 한 번에 로드
@Query("SELECT o FROM Order o LEFT JOIN FETCH o.items")
List<Order> findAllWithItems();
```

결과: 1초 이상 → 100ms 이하 (90% 개선)

도전 2: 데드락 (Deadlock)
┌─────────────────────────┐
│ 문제 상황                 │
├─────────────────────────┤
│ 재고 차감 중 데드락 발생 │
│ - 트랜잭션 A: 상품 X    │
│   - 트랜잭션 B: 상품 Y  │
│ - 나중에 역순 접근       │
│ = 상호 대기로 교착 발생  │
└─────────────────────────┘

원인 분석:
- 동시성 제어 불충분
- 테이블 수준 락의 과도한 사용
- 트랜잭션 순서 보장 미흡

해결책:
```java
// 낙관적 락(Optimistic Locking) 도입
@Entity
public class Inventory {
    @Version
    private Long version;  // 버전 칼럼
    
    private Long quantity;
}

// 재시도 로직
@Transactional(rollbackFor = OptimisticLockingFailureException.class)
public void decreaseInventory(Long productId, Long quantity) {
    try {
        inventory.decrease(quantity);
        inventoryRepository.save(inventory);
    } catch (OptimisticLockingFailureException e) {
        // 재시도 로직
        retryDecrease(productId, quantity);
    }
}
```

결과: 데드락 사건 0건 달성, 동시성 처리량 5배 증가

도전 3: 대용량 데이터 조회 성능
┌─────────────────────────┐
│ 문제 상황                 │
├─────────────────────────┤
│ 1억 개 주문 데이터 조회  │
│ 시 FULL SCAN으로         │
│ 10초 이상 소요           │
│ = 서비스 응답 시간 초과  │
└─────────────────────────┘

해결책:
```sql
-- 복합 인덱스 생성
CREATE INDEX idx_user_date 
ON orders(user_id, created_date DESC);

-- 부분 인덱스 (최근 데이터)
CREATE INDEX idx_recent_orders 
ON orders(created_date DESC) 
WHERE created_date > NOW() - INTERVAL '3 months';

-- 실행 계획 검증
EXPLAIN ANALYZE 
SELECT * FROM orders 
WHERE user_id = 123 
  AND created_date > NOW() - INTERVAL '1 day'
ORDER BY created_date DESC;
```

결과: 10초 → 200ms (98% 개선)

추가 리소스:
- GitHub Repository: https://github.com/your-username/order-system
- API Documentation (Swagger): https://api-docs.example.com
- 관련 블로그 글: "N+1 쿼리 문제 해결하기"
```

---

### 예시 2: 실시간 데이터 분석 플랫폼

```
프로젝트명: Real-time 로그 분석 플랫폼
카테고리: Data Pipeline
한 줄 설명: 초당 100만 건 로그를 실시간으로 수집, 처리, 분석하는 대규모 데이터 시스템

기술 스택:
- Backend: Python FastAPI
- Message Queue: Apache Kafka (5개 파티션)
- Stream Processing: Kafka Streams
- Storage: 
  - TimescaleDB (시계열 데이터)
  - Elasticsearch (로그 검색)
- DevOps: Docker, Kubernetes

정량화된 성과:
1. 처리량: 초당 1,000,000 이벤트
2. 데이터 정합성: 99.99%
3. 스토리지 효율: 80% 용량 절감 (TimescaleDB 압축)
4. 쿼리 성능: 10억 레코드 쿼리 평균 200ms

기술적 도전: Kafka Consumer Lag 증가
문제: 상류 데이터 발생 속도 > 하류 처리 속도
해결: Consumer 그룹 최적화, 파티션 재분배
결과: Lag 95% 감소 → 데이터 신선도 개선
```

---

### 예시 3: 분산 트랜잭션 시스템

```
프로젝트명: Saga 패턴 기반 분산 결제 시스템
카테고리: Distributed Transactions
한 줄 설명: 마이크로서비스 환경에서 ACID 속성을 보장하는 분산 트랜잭션 시스템

기술 스택:
- Backend: Java Spring Cloud
- Database: MySQL (각 서비스별)
- Cache: Redis
- Message Queue: RabbitMQ
- Monitoring: Zipkin (분산 추적)

정량화된 성과:
1. 트랜잭션 성공률: 99.98%
2. 평균 처리 시간: 2초
3. 자동 복구율: 100% (장애 상황)
4. 주문 실패율: 0.02% 이하

기술적 도전: 분산 트랜잭션 일관성
문제: 여러 서비스 간 트랜잭션 일관성 보장 어려움
해결: Saga 패턴 (오케스트레이션 방식) 구현
결과: 모든 분산 트랜잭션 자동 롤백 가능
```

---

## 💡 효과적인 작성 팁

### 1. 정량화 예시 모음

성과를 수치로 표현할 때:

```
성능 개선:
- ✅ "쿼리 응답 시간 1,200ms → 80ms (93% 개선)"
- ✅ "API 응답 시간 45% 단축"
- ✅ "메모리 사용량 60% 감소"

확장성:
- ✅ "동시 접속자 처리량 3배 증가"
- ✅ "초당 처리 요청 수 10배 증가"
- ✅ "데이터베이스 처리 능력 5배 향상"

안정성:
- ✅ "가동률 99.9% 달성"
- ✅ "에러율 0.1% 이하"
- ✅ "데이터 정합성 99.99% 보장"

비용:
- ✅ "클라우드 비용 40% 절감"
- ✅ "인프라 비용 연간 $50,000 절감"
- ✅ "데이터 저장소 용량 80% 절감"
```

### 2. 문제 해결 작성 템플릿

```
문제: [구체적 문제 제목]
├─ 증상: [어떤 문제가 발생했는가]
├─ 원인: [왜 이런 일이 발생했는가]
│  └─ 근본 원인 분석 (Root Cause Analysis)
├─ 해결책: [어떻게 해결했는가]
│  ├─ 기술적 접근
│  ├─ 구현 방식
│  └─ 코드 예시 (선택)
└─ 결과: [해결 후 어떤 개선이 있었는가]
   └─ 정량화된 성과
```

### 3. 좋은 설명 vs 나쁜 설명

❌ 나쁜 예:
- "백엔드를 만들었습니다"
- "데이터베이스를 최적화했습니다"
- "성능이 좋아졌습니다"

✅ 좋은 예:
- "마이크로서비스 아키텍처로 API 서버 3개 구현"
- "N+1 쿼리 문제 해결로 응답 시간 1초 → 100ms"
- "Redis 캐싱 도입으로 DB 부하 60% 감소"

---

## 📊 아키텍처 다이어그램 그리는 팁

ASCII 다이어그램 도구:
- Mermaid: mermaid.js.org (마크다운에 내장 가능)
- Diagrams.net: diagrams.net (무료 온라인)
- Lucidchart: lucidchart.com

예시:
```
User Requests
     ↓
┌─────────────────┐
│  Load Balancer  │
└────────┬────────┘
         ↓
    ┌────┴────┐
    ↓         ↓
┌─────────┐ ┌─────────┐
│ API Srv │ │ API Srv │
└────┬────┘ └────┬────┘
     │           │
     └─────┬─────┘
           ↓
     ┌───────────┐
     │   Cache   │
     │  (Redis)  │
     └─────┬─────┘
           ↓
     ┌───────────┐
     │ Database  │
     │(PostgreSQL)
     └───────────┘
```

---

## 🎯 최종 체크리스트

각 프로젝트마다 다음을 확인하세요:

- [ ] 프로젝트 제목이 명확한가?
- [ ] 기술 스택이 명시되어 있는가?
- [ ] 역할이 구체적으로 설명되어 있는가?
- [ ] 아키텍처 다이어그램이 있는가?
- [ ] 정량화된 성과가 있는가? (반드시!)
- [ ] 기술적 도전이 설명되어 있는가?
- [ ] 어떻게 해결했는지 명확한가?
- [ ] GitHub 링크가 있는가?
- [ ] 코드가 깔끔하고 주석이 있는가?
- [ ] README.md가 상세한가?

포트폴리오의 성공은 **"보이지 않는 곳에서의 논리력"**을 얼마나 잘 보여주는가에 달려 있습니다!
