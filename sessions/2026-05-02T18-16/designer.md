# 🎨 Designer — 회사 정체성(브랜드 톤, 컬러, 타이포그래피)을 기반으로 모든 콘텐츠에 적용될 일관된 비주얼 시스템(스타일 가이드)을 최종 확정하고, 이를 자동화 시스템에 적용할 수 있도록 구체적인 디자인 프롬프트를 정의할 것.

🎨 Designer: 작업 시작합니다.

# ✨ 김애센 비주얼 시스템 최종 확정 및 자동화 프롬프트 정의

CEO의 지시에 따라, 브랜드 정체성(직설적, 데이터 기반, 친근함)을 기반으로 모든 콘텐츠에 적용될 일관된 비주얼 시스템(스타일 가이드)을 확정하고, 이를 자동화 시스템에 적용할 수 있는 구체적인 디자인 프롬프트를 정의합니다.

## 1. 브랜드 핵심 요소 정의 (Foundation)

브랜드 톤에 맞춰 신뢰감, 명확성, 친근함을 동시에 전달하는 컬러, 타이포그래피, 스타일을 확정합니다.

### 1.1. 컬러 팔레트 (Color Palette)

데이터 기반의 신뢰감과 친근함을 동시에 확보하기 위해, 메인 컬러는 신뢰감을 주는 딥 블루 계열을 사용하고, 보조 컬러는 명확성과 활력을 더하는 옐로우/오렌지 계열을 사용합니다.

| 용도 | 색상 이름 | HEX 코드 | RGB | CMYK (인쇄용) | 역할 및 톤 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Primary (신뢰)** | Deep Navy Blue | `#1A237E` | (26, 35, 126) | 85, 75, 35, 50 | 브랜드 핵심, 제목, 주요 섹션 배경 (신뢰) |
| **Secondary (활력)** | Warm Gold | `#FFC107` | (255, 193, 7) | 0, 25, 85, 0 | 강조, 핵심 데이터, CTA 버튼 (친근함, 주목) |
| **Accent (명확성)** | Light Gray | `#F5F5F5` | (245, 245, 245) | 0, 0, 0, 5 | 배경, 본문 영역 (가독성 극대화) |
| **Text (본문)** | Dark Charcoal | `#212121` | (33, 33, 33) | 70, 65, 65, 90 | 본문 텍스트 (최대 가독성) |

### 1.2. 타이포그래피 시스템 (Typography System)

직설적이고 데이터 기반의 톤을 유지하기 위해 산세리프 계열 중 명료하고 굵기가 명확한 폰트를 사용합니다.

| 용도 | 폰트명 | 스타일 | HEX/코드 | 역할 및 톤 |
| :--- | :--- | :--- | :--- | :--- |
| **Headlines (제목)** | Pretendard Bold | Bold (700) | N/A | 강력한 주장, 주제 제시 (직설적, 권위) |
| **Subheadings (소제목)** | Pretendard Medium | Medium (500) | N/A | 내용 분류, 섹션 구분 (명확성) |
| **Body Text (본문)** | Pretendard Regular | Regular (400) | N/A | 정보 전달, 가독성 (친근함, 안정감) |

### 1.3. 레이아웃 및 스타일 가이드 (Layout & Style Guide)

**A. 포스팅 레이아웃 (Long-form Content)**

*   **전체 배경:** `Light Gray (#F5F5F5)`
*   **본문 컨테이너:** `Dark Charcoal (#212121)` 배경 위에 배치하여 대비를 극대화.
*   **제목 (H1):** `Deep Navy Blue (#1A237E)` 배경 박스 처리, `Pretendard Bold` (크기: 32pt).
*   **소제목 (H2/H3):** `Deep Navy Blue (#1A237E)` 텍스트, `Pretendard Medium` (크기: 20pt).
*   **데이터 강조 박스 (Key Takeaways):** `Warm Gold (#FFC107)` 배경에 `Dark Charcoal` 텍스트로 핵심 수치 강조. (여기에 데이터 기반의 시각적 신뢰 부여)
*   **인용구 박스:** `Light Gray` 배경에 얇은 `Deep Navy Blue` 테두리.

**B. 썸네일 컨셉 표준화 (Thumbnail Concept Standardization)**

썸네일은 클릭률(CTR)을 극대화하기 위해 **'데이터 제시'**와 **'인물 중심'**을 결합합니다. 3가지 컨셉을 표준화합니다.

| 컨셉 | 제목/목표 | 레이아웃 및 시각 요소 | 핵심 색상 조합 | 디자인 프롬프트 키워드 (AI용) |
| :--- | :--- | :--- | :--- | :--- |
| **컨셉 1: 데이터 집중형 (Data Focus)** | 핵심 수치 강조 | 왼쪽 1/3: 핵심 숫자 (Warm Gold 배경), 오른쪽 2/3: 인물 또는 그래프 시각화. | `#1A237E` (배경), `#FFC107` (강조) | `Data visualization, bold numbers, professional, deep navy blue background, warm gold accent, clean infographic style, high contrast.` |
| **컨셉 2: 인물/권위형 (Authority Focus)** | 크리에이터 신뢰 구축 | 중앙: 인물 얼굴 클로즈업 (친근한 표정), 주변에 텍스트 오버레이. | `#212121` (텍스트), `#1A237E` (배경) | `Close-up portrait of a friendly expert, direct eye contact, professional, deep navy blue cinematic lighting, clear bold text overlay.` |
| **컨셉 3: 질문/해결형 (Problem/Solution)** | 문제 제기 및 해결책 제시 | 상단: 질문 형태의 큰 텍스트 (Warm Gold), 하단: 해결책을 상징하는 아이콘 및 간결한 텍스트. | `#F5F5F5` (배경), `#1A237E` (텍스트) | `Problem-solution graphic, large question mark, clear solution path, minimalist design, light gray background, navy blue text.` |

## 2. 자동화 시스템 적용을 위한 디자인 프롬프트 정의

위에서 확정된 비주얼 시스템을 기반으로, 이미지 생성 AI에 바로 적용할 수 있는 구체적인 프롬프트를 정의합니다.

### 2.1. 포
