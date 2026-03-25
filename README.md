# dev


### 구분 : AssetFlow

🗄️ 가계부 데이터베이스 테이블 설계 (ERD 구조)
1. Assets (자산 테이블)
- 사용자의 현금, 은행 계좌, 카드 등 돈이 머무는 곳을 정의합니다.

| 필드명 (Column) | 타입 (Type) | 제약 조건 (Constraints) | 설명 |
|:---|:---|:---|:---|
| `id` | SERIAL | PK | 자산 고유 번호 (자동 증가) |
| `name` | VARCHAR(50) | NOT NULL | 자산 명칭 (예: 신한은행, 현대카드) |
| `type` | VARCHAR(20) | NOT NULL | 자산 종류 (BANK, CARD, CASH) |
| `balance` | NUMERIC(15,2) | DEFAULT 0 | 현재 잔액 |
| `is_deleted` | BOOLEAN | DEFAULT FALSE | **삭제 여부 (Soft Delete)** |
| `created_at` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | **등록 일자** |
| `updated_at` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | **수정 일자** |

2. Categories (카테고리 테이블)
- 지출의 분류와 소비 목표(예산)를 관리합니다.

| 필드명 (Column) | 타입 (Type) | 제약 조건 (Constraints) | 설명 |
|:---|:---|:---|:---|
| `id` | SERIAL | PK | 카테고리 고유 번호 |
| `name` | VARCHAR(50) | NOT NULL, UNIQUE | 분류 명칭 (예: 식비, 쇼핑) |
| `budget` | NUMERIC(15,2) | DEFAULT 0 | 월간 목표 예산 |
| `is_deleted` | BOOLEAN | DEFAULT FALSE | **삭제 여부 (Soft Delete)** |
| `created_at` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | **등록 일자** |
| `updated_at` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | **수정 일자** |

3. Histories (거래 내역 테이블)
- 실제 수입과 지출이 일어나는 모든 이벤트를 기록하는 핵심 테이블입니다.

| 필드명 (Column) | 타입 (Type) | 제약 조건 (Constraints) | 설명 |
|:---|:---|:---|:---|
| `id` | BIGSERIAL | PK | 내역 고유 번호 (대용량 대응) |
| `date` | DATE | NOT NULL | 거래 발생 일자 |
| `asset_id` | INTEGER | FK (Assets.id) | 사용된 자산 ID |
| `category_id` | INTEGER | FK (Categories.id) | 분류 ID |
| `description` | TEXT | NOT NULL | 거래 상세 내용 |
| `amount` | NUMERIC(15,2) | NOT NULL | 거래 금액 (지출 -, 수입 +) |
| `type` | VARCHAR(10) | CHECK (type IN ('IN', 'EX')) | INCOME(수입) / EXPENSE(지출) 구분 |
| `is_deleted` | BOOLEAN | DEFAULT FALSE | **삭제 여부 (Soft Delete)** |
| `created_at` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | **등록 일자** |
| `updated_at` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | **수정 일자** |
