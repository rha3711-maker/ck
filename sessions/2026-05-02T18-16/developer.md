# 💻 Developer — Designer가 정의한 비주얼 시스템을 기반으로, 콘텐츠 주제(Business/Researcher가 제공한 데이터)에 따라 이미지를 자동으로 생성하고 최적화하는 API 및 자동화 파이프라인의 초기 설계(Workflow)를 시작할 것.

💻 Developer: 작업 시작합니다.

# ✨ 이미지 생성 및 최적화 자동화 파이프라인 초기 설계 (Workflow)

Designer가 정의한 비주얼 시스템을 기반으로, 콘텐츠 주제에 맞는 이미지를 자동으로 생성하고 최적화하기 위한 API 및 자동화 파이프라인의 초기 설계(Workflow)를 정의합니다.

## 1. 시스템 목표
**목표:** 콘텐츠 주제(텍스트) 입력 $\rightarrow$ 브랜드 스타일 기반 이미지 프롬프트 자동 생성 $\rightarrow$ 고품질 이미지 생성 $\rightarrow$ 블로그 포스팅에 최적화된 이미지 출력

## 2. 구성 요소 (Components)

| 구성 요소 | 역할 | 기술 스택 (예시) |
| :--- | :--- | :--- |
| **Input Handler** | 사용자 또는 데이터 소스에서 콘텐츠 주제(키워드) 및 포스팅 맥락을 입력받음. | Python Scripting |
| **Prompt Generator (Core Logic)** | 입력된 주제와 Designer의 비주얼 시스템(컬러, 톤, 폰트 규칙)을 조합하여 AI 이미지 생성용 상세 프롬프트를 동적으로 생성. | LLM (Self) + Custom Prompt Templates |
| **Image Generation API** | 생성된 프롬프트를 받아 실제 이미지를 생성하는 외부 서비스 호출. | DALL-E 3 API / Stable Diffusion API |
| **Post-Processor (Optimizer)** | 생성된 이미지를 분석하여 브랜드 스타일(컬러 팔레트, 명암)에 맞게 후처리(색 보정, 크기 조정)를 수행. | OpenCV / Pillow (PIL) |
| **Output Manager** | 최종 이미지를 지정된 폴더 구조에 저장하고, 포스팅에 삽입할 최종 파일명 및 메타데이터를 관리. | File System Operations |

## 3. 자동화 워크플로우 (Workflow)

다음은 콘텐츠 주제 입력 시 자동화되는 단계별 흐름입니다.

**Step 1: 주제 입력 (Input)**
*   **Action:** `Input Handler`에 포스팅할 주제 (예: "40대 직장인을 위한 투자 전략") 및 포스팅의 목적(예: 신뢰감 있는 데이터 제시)을 입력합니다.

**Step 2: 프롬프트 생성 (Prompt Generation)**
*   **Action:** `Prompt Generator`가 입력된 주제를 분석하고, **Designer의 비주얼 시스템**을 참조하여 AI 이미지 생성에 최적화된 상세 프롬프트를 생성합니다.
    *   *핵심:* 프롬프트에는 반드시 `#1A237E` (Deep Navy Blue) 계열의 톤, 명확한 데이터 시각화 스타일, 그리고 `Pretendard`와 같은 깔끔한 타이포그래피 느낌을 묘사하도록 지시를 포함해야 합니다.

**Step 3: 이미지 생성 (Generation)**
*   **Action:** 생성된 프롬프트를 `Image Generation API`로 전송하여 원본 이미지를 생성합니다.

**Step 4: 이미지 최적화 (Optimization)**
*   **Action:** 생성된 원본 이미지를 `Post-Processor`로 전달합니다.
    *   **Optimization Rule 1 (Color Mapping):** 이미지의 색상 톤을 `Primary (Deep Navy Blue)`와 `Accent (Warm Gold)` 팔레트에 맞춰 보정합니다.
    *   **Optimization Rule 2 (Aspect Ratio):** 워드프레스 포스팅에 적합한 이미지 비율(예: 16:9 또는 4:3)로 크기를 조정합니다.
    *   **Optimization Rule 3 (Clarity):** 텍스트가 들어갈 영역을 확보하고 배경을 명확하게 만듭니다.

**Step 5: 저장 및 출력 (Output)**
*   **Action:** 최적화된 이미지를 `Output Manager`를 통해 `sessions/images/[주제]/[파일명].webp` 경로에 저장합니다.

## 4. API 통합 설계 (Conceptual API Flow)

이 파이프라인은 Python 기반으로 설계하며, 주요 API 호출은 다음과 같습니다.

1.  **`generate_prompt(topic, style_guide)`** $\rightarrow$ (Internal LLM Call) $\rightarrow$ Image Prompt String
2.  **`create_image(prompt, style_params)`** $\rightarrow$ (External API Call: e.g., DALL-E) $\rightarrow$ Raw Image Data
3.  **`optimize_image(raw_image, color_palette, aspect_ratio)`** $\rightarrow$ (Local Processing: Pillow/OpenCV) $\rightarrow$ Optimized Image File

## 5. 다음 단계 제안
다음 단계로, 이 설계에 기반하여 **Step 2 (Prompt Generator)의 구체적인 프롬프트 템플릿**을 작성하고, **Step 4 (Post-Processor)에 필요한 실제 이미지 보정 코드를 예시**로 작성하여 실행 가능한 스크립트 초안을 제공하겠습니다.

📊 평가: 진행중 — 시스템의 목표와 흐름이 명확하게 정의되었으며, 다음 단계는 실제 코드로 전환하는 데 집중해야 함
📝 다음 단계: Prompt Generator를 위한 비주얼 시스템 기반의 상세 프롬프트 템플릿을 작성하고, Post-Processor의 초기 보정 함수 예시를 제공합니다.
