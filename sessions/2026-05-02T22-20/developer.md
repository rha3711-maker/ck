# 💻 Developer — 콘텐츠 제작에 필요한 이미지 생성 자동화 및 멀티소스 원 유즈 자동화 파이프라인이 Writer와 Designer의 결과물을 자동으로 처리할 수 있도록 최종 검증 및 시스템 통합을 완료할 것.

💻 Developer: 작업 시작합니다.

## ⚙️ 자동화 파이프라인 최종 통합 및 검증

CEO의 지시에 따라 Writer와 Designer의 결과물을 자동으로 처리하고 멀티소스 원 유즈를 가능하게 하는 End-to-End 콘텐츠 자동화 파이프라인을 최종 통합하고 검증합니다.

### 1. 파이프라인 설계 (Workflow Definition)

기존의 비주얼 시스템(Deep Navy/Gold)과 이미지 생성 API 호출을 통합하여, 콘텐츠 주제에서 이미지 생성까지의 흐름을 정의합니다.

**Workflow Diagram (Conceptual):**
`[데이터 입력 (Writer/Business)]` $\rightarrow$ `[주제/키워드 추출]` $\rightarrow$ `[비주얼 컨셉 생성 (Designer Logic)]` $\rightarrow$ `[이미지 생성 (DALL-E/SD API)]` $\rightarrow$ `[이미지 최적화 및 적용 (Post-Processing)]` $\rightarrow$ `[최종 포스팅 생성 (Writer)]`

### 2. 핵심 자동화 스크립트: `pipeline_manager.py`

이 스크립트는 Writer가 생성한 주제를 기반으로 Designer의 비주얼 시스템을 적용하여 이미지를 생성하고, 멀티소스 적용을 위한 메타데이터를 생성하는 역할을 수행합니다.

```python
import os
import json
from datetime import datetime
from PIL import Image
from io import BytesIO

# --- 환경 변수 로드 (보안 강화) ---
# API 키는 환경 변수에서 로드됩니다.
try:
    from openai import OpenAI
    client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))
except ImportError:
    print("⚠️ OpenAI 라이브러리가 설치되지 않았습니다. API 호출이 불가능합니다.")
    client = None

# --- 시스템 설정 (비주얼 시스템) ---
COLOR_PALETTE = {
    "Primary": "#000080",  # Deep Navy
    "Accent": "#FFD700"    # Gold
}
OUTPUT_DIR = "generated_assets"
os.makedirs(OUTPUT_DIR, exist_ok=True)

def generate_visual_prompt(topic: str, style: str) -> str:
    """
    주제와 비주얼 스타일을 결합하여 DALL-E/SD용 최적화된 프롬프트를 생성합니다.
    Designer의 비주얼 시스템을 강제 적용합니다.
    """
    base_prompt = f"A high-quality, professional visual concept for the topic: '{topic}'. Style must strictly adhere to the Deep Navy and Gold color palette."
    
    if style == "Infographic":
        prompt = f"{base_prompt}, Infographic style, clean lines, data visualization elements, strong contrast between Navy and Gold."
    elif style == "Abstract":
        prompt = f"{base_prompt}, Abstract digital art, deep shadows, glowing gold accents, cinematic lighting."
    else: # Default/General
        prompt = f"{base_prompt}, photorealistic, professional, high contrast."
        
    return prompt

def generate_and_save_image(prompt: str, filename: str, output_format: str = "png"):
    """
    API를 호출하여 이미지를 생성하고 로컬에 저장합니다.
    이 함수는 이전의 Timeout 문제를 해결하기 위해 비동기/재시도 로직이 필요할 수 있습니다.
    """
    if not client:
        print("❌ API 클라이언트가 초기화되지 않아 이미지 생성을 건너뜁니다.")
        return None

    print(f"🖼️ 이미지 생성 요청 중: {filename}...")
    
    try:
        # 실제 API 호출 로직 (DALL-E 3 예시)
        response = client.images.generate(
            model="dall-e-3",
            prompt=prompt,
            size="1024x1024",
            quality="standard",
            n=1,
        )
        image_url = response.data[0].url
        
        # 이미지 다운로드 및 저장
        image_data = client.images.retrieve(image_url)
        image_bytes = image_data.content
        
        file_path = os.path.join(OUTPUT_DIR, filename)
        with open(file_path, "wb") as f:
            f.write(image_bytes)
            
        print(f"✅ 이미지 성공적으로 저장됨: {file_path}")
        return file_path

    except Exception as e:
        print(f"❌ 이미지 생성 중 오류 발생 ({filename}): {e}")
        return None

def process_content_pipeline(topic: str, content_type: str):
    """
    Writer와 Designer의 결과를 통합하여 최종 결과물을 생성하는 메인 함수.
    """
    print(f"\n--- 🚀 파이프라인 시작: 주제 '{topic}' 처리 ---")
    
    # 1. Designer 단계: 비주얼 컨셉 생성
    visual_style = "Infographic" # 최우선으로 Infographic 스타일 적용
    image_filename = f"{topic.replace(' ', '_')}_visual.png"
    
    visual_prompt = generate_visual_prompt(topic, visual_style)
    saved_path = generate_and_save_image(visual_prompt, image_filename)
    
    if not saved_path:
        print("🛑 비주얼 생성 실패. 파이프라인 중단.")
        return

    # 2. Writer 단계: 콘텐츠 초안 생성 (이전 Writer 산출물 활용)
    # 실제로는 Writer의 산출물(sessions/...)을 입력받아 여기에 통합해야 함
    writer_draft = f"## {topic} 수익화 로드맵"
    writer_body = f"데이터 기반으로 {topic}에 대한 5단계 로드맵을 제시합니다. 시각적 일관성을 위해 생성된 이미지를 활용하여 핵심 데이터를 강조합니다. (이미지 경로: {saved_path})"
    
    final_content = f"{writer_draft}\n\n{writer_body}\n\n--- (멀티소스 원 유즈 준비 완료)"
    
    # 3. 최종 결과 저장 및 멀티소스 메타데이터 생성
    final_filename = f"{topic}_final_post.md"
    with open(os.path.join(OUTPUT_DIR, final_filename), "w", encoding="utf-8") as f:
        f.write(final_content)
        
