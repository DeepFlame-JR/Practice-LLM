# Train LLM

Reference
- https://medium.com/@murtuza753/using-llama-2-0-faiss-and-langchain-for-question-answering-on-your-own-data-682241488476

### Introduction
- LLaMa2
  - 2조개의 token으로 Pretrain한 Fine-tuning 모델
  - 7 to 70 billion parameteres
- LangChain
  - Language Model (특히 LLM) 을 사용하는 어플리케이션을 개발하는 것을 도와주는 오픈소스  framework

### Flow
<img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*mdzyMcDhL0YLtgtfw7eaaA.png" width="500">

1. 모델 파이프라인 초기화: Pretrain 모델인 Llama-2-7b-chat-hf를 HuggingFace Transformer를 통해서 text-generation pipeline을 초기화
1. 데이터 수집: 텍스트 형식의 임의 소스에서 문서 로더로 데이터를 로드
1. Chunk로 분할: 로드된 텍스트를 더 작은 Chunk로 분할. (언어 모델은 제한된 양의 텍스트를 처리할 수 있기 때문에 작은 Chunk로 생성)
1. 임베딩 만들기: Text Chunk를 임베딩으로 변환
1. 임베딩을 Vector Store에 로드: `FAISS` 사용. (기존 DB와 비교해서 텍스트 임베딩을 사용한 유사성 검색에서 매우 잘 수행)
1. 메모리 활성화: 채팅 기록을 새 질문과 결합하여 하나의 독립적인 질문으로 전환하는 후속 질문을 할 수 있는 기능을 활성화하는 데 매우 중요
1. Quey data: Vector Store에 저장된 관련 정보를 임베딩을 사용하여 검색
1. 답변 생성: 언어 모델을 사용하여 답변을 생성하는 질문 답변 체인에 전달
