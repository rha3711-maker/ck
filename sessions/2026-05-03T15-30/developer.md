# 💻 Developer — 이미지 생성 자동화 파이프라인 및 멀티소스 원 유즈 자동화에 필요한 초기 API 및 데이터 파이프라인 구조를 설계하고 구현 계획을 수립하세요.

💻 Developer: 작업 시작합니다.

## 1. 이미지 생성 자동화 및 멀티소스 원 유즈 자동화 파이프라인 설계 및 구현 계획

CEO의 지시사항에 따라, 시각적 일관성(Deep Navy/Gold)을 유지하며 콘텐츠 제작의 효율성을 극대화하기 위한 자동화 파이프라인의 초기 구조를 설계하고 구현 계획을 수립합니다.

### A. 파이프라인 목표 및 구조 개요

**목표:** 콘텐츠 주제 입력 $\rightarrow$ 브랜드 스타일 기반 이미지 자동 생성 $\rightarrow$ 이미지 최적화 $\rightarrow$ 멀티소스 원 유즈 데이터 통합 및 최종 포스팅 준비.

**핵심 구성 요소:**
1.  **Input Layer (입력 계층):** 주제, 키워드, 포스팅 내용 (Writer/Business 에이전트 산출물).
2.  **Vision & Prompt Engine (비전 엔진):** 브랜드 톤(Deep Navy/Gold)을 반영한 고정 프롬프트 템플릿 관리.
3.  **Image Generation Module (이미지 생성 모듈):** DALL-E/Stable Diffusion API 연동 및 이미지 생성/최적화.
4.  **Orchestration Layer (오케스트레이션):** 전체 프로세스를 관리하고 결과물을 통합하는 스크립트 (Python 기반).
5.  **Output & Integration Layer (출력 통합):** 생성된 이미지를 원본 텍스트와 연결하여 최종 포스팅 형식으로 출력.

### B. 기술 스택 및 API 설계

| 구성 요소 | 기술/API | 역할 | 비고 |
| :--- | :--- | :--- | :--- |
| **언어** | Python 3.x | 전체 워크플로우 관리 | 자동화 스크립트의 핵심 |
| **API 연동** | OpenAI/Stability AI API | 이미지 생성 | 스타일 일관성 유지 |
| **데이터 처리** | Pandas / JSON | 멀티소스 데이터 통합 및 관리 | 원 유즈 데이터 구조화 |
| **스타일 관리** | Configuration File (YAML/JSON) | 컬러 팔레트, 프롬프트 템플릿 관리 | 비주얼 일관성 보장 |

### C. 단계별 구현 계획 (Action Plan)

**Phase 1: Vision & Prompt Engine 구축 (스타일 고정)**
1.  **Action:** Deep Navy/Gold 기반의 마스터 프롬프트 템플릿 파일 (`style_prompts.json`)을 정의합니다. (Designer 에이전트의 결과물 참조)
2.  **Action:** API 키 환경변수 설정 및 테스트 환경을 확보합니다. (보안 준수)

**Phase 2: Image Generation Module 구현 (핵심 기능)**
1.  **Action:** 주제 입력(Input Layer)을 받아, 정의된 템플릿에 동적으로 키워드를 삽입하는 함수를 구현합니다.
2.  **Action:** API를 호출하여 이미지를 생성하고, 생성된 이미지를 지정된 형식(예: 16:9 비율, 특정 해상도)으로 최적화하는 모듈을 개발합니다.

**Phase 3: Multi-source 원 유즈 통합 (자동화 완성)**
1.  **Action:** Writer 및 Researcher 에이전트의 산출물(포스팅 초안, 데이터)을 입력으로 받아, 이미지 생성 결과를 해당 텍스트의 맥락에 맞게 배치하는 통합 스크립트를 작성합니다.
2.  **Action:** 모든 과정이 성공적으로 완료되었는지 검증하는 에러 핸들링 로직을 추가합니다.

### D. 실행 가능한 초기 코드 스켈레톤 (Python)

다음은 Phase 1과 2의 핵심 로직을 담은 Python 스켈레톤입니다. 실제 API 키 설정은 환경변수를 통해 이루어져야 합니다.

```python
import os
import json
from typing import Dict, Any
import requests

# --- 1. Configuration & Style Management ---
# 환경변수에서 API 키를 로드한다고 가정
# os.environ.get("OPENAI_API_KEY") 

STYLE_CONFIG = {
    "palette": ["#000080", "#FFD700"],  # Deep Navy, Gold
    "aspect_ratio": "16:9",
    "base_prompt_template": (
        "A high-quality, professional image for a business topic. "
        "Style: Cinematic, Deep Navy and Gold color palette. "
        "Focus on data visualization and trust."
    )
}

def generate_image_prompt(topic: str, context: str) -> str:
    """주제와 컨텍스트를 기반으로 API에 전달할 최종 프롬프트를 생성합니다."""
    # 비주얼 일관성을 위한 스타일 정보를 프롬프트에 주입
    style_instruction = f"Apply the color scheme {STYLE_CONFIG['palette']} and maintain a professional, data-driven aesthetic."
    
    final_prompt = f"{STYLE_CONFIG['base_prompt_template']} Subject: {topic}. Context Detail: {context}. {style_instruction}"
    return final_prompt

def call_image_api(prompt: str, model: str = "dall-e-3", quality: str = "hd") -> str:
    """DALL-E 3 API를 호출하여 이미지를 생성하고 Base64로 반환합니다 (Mock)."""
    print(f"-> API Call: Generating image with prompt: {prompt[:100]}...")
    # 실제 API 호출 로직은 여기에 구현됩니다.
    # 예시 반환 값 (실제 구현 시 API 응답을 사용)
    return f"BASE64_IMAGE_DATA_FOR_{hash(prompt)}"

def process_content_pipeline(topic: str, content_text: str, source_data: Dict[str, Any]) -> Dict[str, Any]:
    """전체 콘텐츠 파이프라인을 실행하고 결과를 통합합니다."""
    print(f"\n--- Pipeline Start for Topic: {topic} ---")
    
    # 1. 프롬프트 생성
    image_prompt = generate_image_prompt(topic, content_text)
    
    # 2. 이미지 생성
    image_data = call_image_api(image_prompt)
    print(f"-> Image Generated. Data ID: {image
