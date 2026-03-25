# dev


### 구분 : AssetFlow

🗄️ 가계부 데이터베이스 테이블 설계 (ERD 구조)
1. Assets (자산 테이블)
- 사용자의 현금, 은행 계좌, 카드 등 돈이 머무는 곳을 정의합니다.

| 필드명 (Column) | 타입 (Type) | 필수 | 설명 | 비고 |
|:--- |:--- |:---:|:--- |:--- |
| `id` | String | ✅ | 자산 고유 식별자 | PK (Primary Key) |
| `name` | String | ✅ | 자산 이름 | 예: 신한은행, 현대카드, 현금 |
| `type` | String | ✅ | 자산 종류 | 예: BANK, CARD, CASH |
| `balance` | Number | ✅ | 현재 잔액 | 실시간 잔액 (초기값 + 수입 - 지출) |
| `memo` | String | - | 자산 관련 메모 | 계좌번호나 카드 결제일 등 |
| `delYn` | String | - | 삭제여부 | 계좌번호나 카드 결제일 등 |

2. Categories (카테고리 테이블)
- 지출의 분류와 소비 목표(예산)를 관리합니다.

| 필드명 (Column) | 타입 (Type) | 필수  | 설명          | 비고                  |
| :----------- | :-------- | :-: | :---------- | :------------------ |
| `id`         | String    |  ✅  | 카테고리 고유 식별자 | PK (Primary Key)    |
| `name`       | String    |  ✅  | 분류 명칭       | 예: 식비, 교통, 쇼핑, 주거   |
| `budget`     | Number    |  ✅  | 월간 목표 예산    | 0원 이상 (예산 대비 분석용)   |
| `icon`       | String    |  -  | UI용 아이콘     | 예: 🍔, 🚗, 🛍️      |
| `is_active`  | Boolean   |  ✅  | 사용 여부       | 카테고리 삭제 대신 비활성화 처리용 |
| `delYn` | String | - | 삭제여부 | 계좌번호나 카드 결제일 등 |

3. Histories (거래 내역 테이블)
- 실제 수입과 지출이 일어나는 모든 이벤트를 기록하는 핵심 테이블입니다.

| 필드명 (Column) | 타입 (Type) | 필수 | 설명 | 비고 |
|:--- |:--- |:---:|:--- |:--- |
| `id` | String | ✅ | 내역 고유 식별자 | PK (Primary Key) |
| `date` | String | ✅ | 거래 날짜 | YYYY-MM-DD (ISO 8601) |
| `asset_id` | String | ✅ | 사용된 자산 ID | FK (Assets.id 참조) |
| `category_id` | String | ✅ | 분류 ID | FK (Categories.id 참조) |
| `description` | String | ✅ | 거래 상세 내용 | 예: 스타벅스 강남점 |
| `amount` | Number | ✅ | 거래 금액 | 수입(+), 지출(-) |
| `type` | String | ✅ | 거래 구분 | INCOME(수입), EXPENSE(지출) |
| `delYn` | String | - | 삭제여부 | 계좌번호나 카드 결제일 등 |
