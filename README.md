# LLM-project
LLM 파인 튜닝 및 파인 튜닝된 모델 사용해보기 (EEVE-Korean, Alpaca, gemma 모델)


## 1. 한국어 파인 튜닝 모델
Ollama 프레임워크로 한국어 학습 파인튜닝 모델 (EEVE-Korean) 모델 받아서 로컬 환경에서 실행하기

<img width="803" alt="image" src="https://github.com/yammayamm/LLM-project/assets/49015100/4f045ac1-aa6f-4500-83ad-adf8e78e8b92">


### 1) HuggingFace-Hub 및 ollama 설치
```
pip install huggingface-hub
```
https://ollama.com/download

### 2) 모델 다운로드
```
huggingface-cli download \
  heegyu/EEVE-Korean-Instruct-10.8B-v1.0-GGUF \
  ggml-model-Q5_K_M.gguf \
  --local-dir 본인의_컴퓨터_다운로드폴더_경로 \
  --local-dir-use-symlinks False
```
### 3) 다운로드한 gguf 파일과 같은 위치에 [Modelfile](https://github.com/yammayamm/LLM-project/blob/main/Modelfile) 만들기

### 4) Ollama 실행
```
ollama create EEVE-Korean-10.8B -f EEVE-Korean-Instruct-10.8B-v1.0-GGUF/Modelfile
ollama run EEVE-Korean-10.8B:latest
```

## 2. Alpaca LoRa 파인튜닝

Parameter-Efficient Fine-Tuning (PEFT): 모델의 모든 파라미터를 미세 조정하지 않고도 사전 학습된 언어 모델(PLM)을 효율적으로 적용할 수 있다.

PEFT 방식은 적은 모델 파라미터만 미세 조정하므로 계산 및 저장 비용을 크게 절감할 수 있다.

PEFT 기법 중 하나인 LoRA 방식으로 파인 튜닝을 적용해본다.

[code](https://github.com/yammayamm/LLM-project/blob/main/LLaMa_Alpaca_LoRA_%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%90%E1%85%B2%E1%84%82%E1%85%B5%E1%86%BC.ipynb)

### 1) 파인튜닝할 데이터 다운로드

https://github.com/Beomi/KoAlpaca/raw/main/ko_alpaca_data.json

### 2) 하이퍼 파라미터 튜닝

num_epochs, learning_rate, batch_size, max_step 등 수정하며 진행

## 3. Google gemma 파인튜닝

KoAlpaca 한국어 데이터셋을 구글 젬마 모델의 Instruction 포맷으로 변환하고 PEFT (QLoRA) 기법으로 파인튜닝하는 과정

[code](https://github.com/yammayamm/LLM-project/blob/main/gemma_finetuning_koalpaca.ipynb)

### 1) 파인튜닝할 데이터 다운로드

instruction: 유저의 질문

input: 추가 text

ouput: 모델의 답

text: 모델에 들어가는 text

### 2) Gemma 데이터셋 포맷팅

<start_of_turn>user

...<end_of_turn>

<start_of_turn>model

...<end_of_turn>

### 3) 모델 로드 및 튜닝

BitsAndBytes 설정, 모델과 토크나이저 import (gemma-2b)

### 4) 인코딩

토크나이저 적용해서 인코딩 (256 size)

### 5) 파인튜닝
