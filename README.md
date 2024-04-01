# korea_paper_search_chatbot
 ## This is a Korean paper search chatbot project
 
We have fine-tuned the LDCC-SOLAR-10.7B model to create a chatbot that can search for papers in a chat format.

LDCC-SOLAR-10.7B 모델을 fine-tuning하여 채팅 형식으로 논문을 찾아주는 챗봇을 만들었습니다.

![image](https://github.com/ahdgks/koala_project/assets/135018581/95c121e1-b988-4523-a5ec-3b20979e0152)

---
## Table of Contents
Dataset
- 1. load_dataset.ipynb (aihub 데이터 다운로드 후 csv로 만드는 파일)
- 2. preprocessing_dataset.ipynb (한자 제거, 이름 전처리)
- 3. question_dataset_gpt.ipynb (gpt4.0로 fine_tuning에 사용할 데이터셋 생성)
     이때 원래 1900개를 만드려고 했으나 중간에 오류로 1682개 데이터가 만들어짐
    
lora_Training
- 1. lora_fine_tuning.ipynb

RAG
- 1. vector_db_make.ipynb
     
Inference
-1. Inference.ipynb 

Streamlit
-1. paper_streamlit.ipynb


---
## Dataset

### Dataset 개요
이 프로젝트는 open-source LLM인 solar 10.7B를 한국어 사용이 가능하게 fine-tuning한 LDCC-SOLAR-10.7B 모델을 사용했습니다. 이를위해 한국어로된 논문 데이터셋을 사용했습니다. 목표는 관련된 논문을 찾아달라는 질문을 하면 관련된 논문에 대한 제목, 저자, 발행년도, 짧은 내용을 출력하는 것입니다. 외국은 수많은 데이터가 공개되어있기 때문에 관련 서비스도 존재하지만 한국의 경우는 데이터 공개의 제한으 성능의 아쉬움은 존재합니다. 


### Dataset 출처
사용된 데이터는 AI hub의 논문자료 요약 데이터셋 중 학술논문-섹션요약 데이터 입니다. 해당 데이터는 ai hub에서 사용 허가를 받고 사용해야합니다!

![image](https://github.com/ahdgks/koala_project/assets/135018581/304ffc58-524e-4a3b-b2bd-1018eee02d96)

그 중 doc_id, title, date, author, section_summary_text를 사용했습니다. 해당 데이터셋은 전처리를 통해 RAG에도 사용되었습니다.
해당 데이터셋은 사회과학과 복합학의 비중이 높습니다.

### Fine-tuning 데이터셋
doc_id, title, date, author, section_summary_text를 사용해 답변 형식을 만들고 이에 대한 질문을 GPT 4.0을 사용해 생성했습니다.

아래는 사용한 프롬프트와 만들어진 질문 답변쌍입니다.

![image](https://github.com/ahdgks/koala_project/assets/135018581/f9bdf396-3345-458e-a357-1e80efc08b5a)

![image](https://github.com/ahdgks/koala_project/assets/135018581/e7841774-99a6-4e62-b5c8-1b5cb9cbba23)


## Fine-tuning
GPU 성능 제한 이슈로 lora를 사용했습니다. 

이때 low rank = 8로 사용했습니다.

## RAG 
위의 AI hub 데이터를 전처리하여 RAG에 사용할 데이터 구축을 했습니다. 이를 'intfloat/multilingual-e5-large'를 사용해 임베딩 하고 choma db에 저장했습니다. 

## Streamlit

Streamlit을 이용하여 웹을 구축했고 streamlit 내에서 gpu를 제공하지 않는 문제로 로컬에서만 테스트를 해 보았습니다.
해당 코드를 실행하면 생기는 ko_paper_streamlit.py를 실행하면 웹으로 연결할 수 있는 주소가 나오고 이를 주소창에 넣으면 아래와 같이 웹으로 실행됩니

![image](https://github.com/ahdgks/koala_project/assets/135018581/c2bb91f9-ea23-4eb6-9b26-c711b22a1322)
![image](https://github.com/ahdgks/koala_project/assets/135018581/52e88e14-1439-4ede-8555-f88cad61712d)

