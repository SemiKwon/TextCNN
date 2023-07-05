# TextCNN 모델을 활용한 갈등 인지
**일반적인 갈등 맥락을 인지하는 딥러닝 모델**

2021년 Sungshin_3F 경진대회에서 웹툰 추천 알고리즘에 대해 설명할 때
'자극이 두드러지는 작품을 선호하시나요?'라는 질문에 대해 또래 평가에서
'**자극의 기준**이 무엇인가요?'라는 질문을 받았던 경험이 있습니다.

**의도했던 자극의 정의는 인물 간의 의견 충돌 혹은 다툼**이었는데,
자극의 기준으로 이 외에도 **선정성, 말 그대로의 아픔 등 유저들이 받아들이는 의의가 다를 수 있다**는 점을 깨달았습니다. 이를 보완하고자 딥러닝 모델 프로젝트를 진행하게 되었습니다.

TextCNN을 채택하게 된 이유로는 웹툰 댓글은 길이가 짧고, 그 특징을 추출하는 것이 중요하다고 생각했기 때문입니다.

"**인물 사이의 자극적인 장면이 많은 작품을 선호하시나요?**"
로 질문을 바꾸고, TextCNN 모델의 결과를 기준으로 **DB에 담겨 있는 웹툰 작품들을 재배열**하였습니다. **10명을 대상으로 파일럿 스터디**하여 다시 한번 테스트해본 결과 **초기 애플리케이션에서는 정확도가 60%였던 것이, 80%까지 상승**하는 결과를 얻을 수 있었습니다.

## 📌 데이터셋 구성

추천 대상이 되는 15개의 웹툰의 10화까지 베스트댓글 15개를. 수집 갈등의 표현을 잘 드러내고 있는 텍스트 데이터 100개를 txt 파일로 제작

- **OKT 형태소 분석기로 문장 분해**
- **위의 결과와 KNU 감정 사전을 매칭시켜 감정 점수 계산**
- **계산 결과에 따라 0보다 크면 일반 맥락(0), 0보다 같거나 작으면 갈등 맥락(1)으로 라벨링**

## 📌 딥러닝 모델이 한글을 학습할 수 있도록 데이터셋을 변환
문자를 숫자로 변환하여 컴퓨터가 인식하도록 해야 하는데, 단어나 문장을 벡터로 변환하여 벡터로 끼워넣는 과정을 word embedding(단어 임베딩)이라고 함. 분산 표현을 통해 여러 차원이 조합되어 의미적 유사도를 구할 수 있음.
이 프로젝트에서는 Word2Vec 임베딩을 사용

처음에 '차원'이라는 개념이 어려웠는데, 'I love you'를 예시로 들어보면 I=[1, 0, 0], love=[0, 1, 0], you=[0, 0, 1]의 벡터(행렬)로 나타낼 수 있음. 아래와 같이 중심 단어에 대한 벡터와 주변 단어에 대한 벡터를 교차하여 값을 계산하면 유사도를 파악하 수 있음.

![단어](https://github.com/SemiKwon/TextCNN/assets/76101347/00ba00db-ad49-47a5-83a3-22cb80c315ef)

Word2Vec은 중심 단어와 주변 단어를 통해 단어를 예측하는 방식. Word2Vec에는 두 가지 방법이 있는데 그중 Skip-gram을 선택했음. Skip-gram은 중심 단어에서 주변 단어를 예측하는 방식

![skipgram_dataset](https://github.com/SemiKwon/TextCNN/assets/76101347/4b5fc202-11cc-46f6-867f-9f3ecb5060a0)

![word2vec_renew_6](https://github.com/SemiKwon/TextCNN/assets/76101347/16e929a4-2c67-4acc-802b-f60d46418cf1)

출처 : https://wikidocs.net/22660

## 📌 TextCNN 모델

**1) TextCNN이란?**
* CNN → 이미지 처리 시 스캔하면서 특징을 추출 / TextCNN은 filter가 문장 스캔 → 문맥적 의미 파악 (정보 집약-연산속도 향상-분류 문제에서 좋은 결과) 
* 워드 임베딩 벡터를 입력값으로 투입 → Convolution 연산을 통해 feature map 생성 → 활성화 함수를 통해 feature map을 activation map으로 대응 → max-pooling → 결과를 fully-connected layer의 입력값으로 넣은 뒤 최종 분류

![다운로드](https://github.com/SemiKwon/TextCNN/assets/76101347/8af3323a-5f1f-4c8f-aaf5-2c5d088a5a8d)
)

출처 : IMPLEMENTING A CNN FOR TEXT CLASSIFICATION IN TENSORFLOW

![화면 캡처 2023-07-05 223117](https://github.com/SemiKwon/TextCNN/assets/76101347/68727ecb-61a9-4ebe-91b3-b56a66ea19f2)

위 사진의 구조로 의도한 모델을 설명하자면, 입력값(input)의

출처 :  A Sensitivity Analysis of (and Practitioners’ Guide to) Convolutional Neural Networks for Sentence Classification 
