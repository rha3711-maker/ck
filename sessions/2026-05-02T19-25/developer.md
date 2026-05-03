# 💻 Developer — 이미지 생성 API 연동 및 초기 테스트 환경 구축(DALL-E/SD API 키 설정)을 완료하고, Designer가 정의한 비주얼 시스템(Deep Navy/Gold)을 반영하는 기본 프롬프트 템플릿의 작동 가능성을 검증하라.

💻 Developer: 작업 시작합니다.

## 🖼️ 이미지 생성 API 연동 및 비주얼 시스템 테스트 환경 구축

CEO의 지시에 따라 DALL-E/SD API 연동 및 Deep Navy/Gold 비주얼 시스템을 반영하는 기본 프롬프트 템플릿의 작동 가능성을 검증하기 위한 초기 환경 설정 및 테스트 스크립트를 제공합니다.

### 1. 환경 변수 설정 (API 키 관리)

외부 호출 시 키 노출을 방지하기 위해 환경 변수를 사용합니다. 실제 키는 보안상 이 세션 내에서 직접 설정하거나, 시스템 환경 변수로 설정해야 합니다.

**실행 방법:**
터미널에서 아래 명령어를 실행하여 API 키를 환경 변수로 설정합니다.

```bash
export OPENAI_API_KEY="YOUR_OPENAI_API_KEY"
# 또는 Stable Diffusion 관련 키가 있다면 추가 설정 (예시)
# export SD_API_KEY="YOUR_SD_API_KEY"
```

### 2. 기본 프롬프트 템플릿 정의 (Visual System 반영)

Designer가 정의한 컬러 팔레트(Deep Navy, Gold)와 브랜드 톤(직설적, 데이터 기반, 친근함)을 반영하는 기본 프롬프트 템플릿을 정의합니다.

**파일:** `templates/visual_prompt_template.py`

```python
import os

def generate_visual_prompt(subject: str, style: str = "Deep Navy and Gold Aesthetic") -> str:
    """
    주어진 주제와 비주얼 시스템을 결합하여 이미지 생성을 위한 최적화된 프롬프트를 생성합니다.

    Args:
        subject (str): 이미지의 핵심 주제 (예: '40대 남성의 비즈니스 성공')
        style (str): 적용할 시각적 스타일 (기본값: 'Deep Navy and Gold Aesthetic')

    Returns:
        str: API 호출에 바로 사용 가능한 최종 프롬프트 문자열
    """
    # 브랜드 톤 및 시각 시스템을 프롬프트에 주입
    base_prompt = f"A high-quality, data-driven visualization of: {subject}. "
    visual_context = f"Style: {style}. Cinematic lighting, high contrast, professional, deep navy and gold color palette."
    
    final_prompt = f"{base_prompt} {visual_context}"
    return final_prompt

if __name__ == "__main__":
    # 테스트 실행 예시
    test_subject = "40대 비즈니스 리더의 성공과 데이터 기반 의사결정"
    generated_prompt = generate_visual_prompt(test_subject)
    print("--- 생성된 프롬프트 ---")
    print(generated_prompt)
```

**사용 방법:**
위 스크립트를 실행하여 테스트 프롬프트를 확인합니다.

```bash
python templates/visual_prompt_template.py
```

**예상 출력 (실제 API 연동 전):**
```
--- 생성된 프롬프트 ---
A high-quality, data-driven visualization of: 40대 비즈니스 리더의 성공과 데이터 기반 의사결정. Style: Deep Navy and Gold Aesthetic. Cinematic lighting, high contrast, professional, deep navy and gold color palette.
```

### 3. API 연동 및 테스트 스크립트 (Python 예시)

실제 API 호출을 위한 기본적인 Python 스크립트를 작성합니다. 이 스크립트는 위에서 정의된 템플릿을 사용하여 이미지 생성을 시도합니다.

**파일:** `scripts/test_image_generation.py`

```python
import os
import openai
from templates.visual_prompt_template import generate_visual_prompt

# 환경 변수에서 API 키 로드
try:
    openai.api_key = os.environ["OPENAI_API_KEY"]
except KeyError:
    print("오류: OPENAI_API_KEY 환경 변수가 설정되지 않았습니다. API 키를 설정해주세요.")
    exit(1)

def call_dalle_image(prompt: str, model: str = "dall-e-3"):
    """
    DALL-E API를 호출하여 이미지를 생성하는 함수
    """
    print(f"\n--- DALL-E 호출 시도 (모델: {model}) ---")
    print(f"입력 프롬프트: {prompt[:200]}...")
    
    try:
        response = openai.Image.create(
            model=model,
            prompt=prompt,
            n=1,
            size="1024x1024",
            quality="hd"
        )
        image_url = response['data'][0]['url']
        print("✅ 이미지 생성 성공. URL:", image_url)
        return image_url
    except Exception as e:
        print(f"❌ DALL-E API 호출 중 오류 발생: {e}")
        return None

if __name__ == "__main__":
    # 1. 비주얼 시스템을 적용한 주제 설정
    topic = "40대 남성의 비즈니스 성공과 데이터 기반 의사결정"
    
    # 2. 템플릿을 사용하여 최종 프롬프트 생성
    final_prompt = generate_visual_prompt(topic)
    
    print("===================================================")
    print("✨ 최종 테스트 프롬프트:")
    print(final_prompt)
    print("===================================================")
    
    # 3. 이미지 생성 시도 (API 키가 설정되어 있어야 성공)
    generated_url = call_dalle_image(final_prompt)
    
    if generated_url:
        print("\n🚀 테스트 완료: 이미지 생성 파이프라인의 기본 작동 가능성을 검증했습니다.")
    else:
        print("\n🛑 테스트 실패: API 호출이 실패했거나 키 설정에 문제가 있습니다. 환경 변수를 확인하세요.")
```

### 🚀 실행 방법 요약

1.  **API 키 설정:** 터미널에서 `export OPENAI_API_KEY="YOUR_API_KEY"` 실행.
2.  **파일 구조 확인:** `templates/visual_prompt_template.py`와 `scripts/test_image_generation.py` 파일이 올바르게 저장되었는지 확인.
3.  **스크립트 실행:** `python scripts/test_image_generation.py` 실행.

---
📊 평가: 진행중 — API 키 설정이 필요하여 실제 이미지 생성은 대기 상태입니다.
📝 다음 단계: 환경 변수 설정 후, `scripts/test_image_generation.py`를 실행하여 API 연결 상태를 최종 확인합니다.
