# TextCNN 모델을 활용한 갈등 인지
**일반적인 갈등 맥락을 인지하는 딥러닝 모델**

2021년 Sungshin_3F 경진대회에서 웹툰 추천 알고리즘에 대해 설명할 때
'자극이 두드러지는 작품을 선호하시나요?'라는 질문에 대해 또래 평가에서
'**자극의 기준**이 무엇인가요?'라는 질문을 받았던 경험이 있습니다.

**의도했던 자극의 정의는 인물 간의 의견 충돌 혹은 다툼**이었는데,
자극의 기준으로 이 외에도 **선정성, 말 그대로의 아픔 등 유저들이 받아들이는 의의가 다를 수 있다**는 점을 깨달았습니다. 이를 보완하고자 딥러닝 모델 프로젝트를 진행하게 되었습니다.

**'인물 사이의 자극적인 장면이 많은 작품을 선호하시나요?'**로 질문을 바꾸고, TextCNN 모델의 결과를 기준으로 **DB에 담겨 있는 웹툰 작품들을 재배열**하였습니다. **10명을 대상으로 파일럿 스터디**하여 다시 한번 테스트해본 결과 **초기 애플리케이션에서는 정확도가 60%였던 것이, 80%까지 상승**하는 결과를 얻을 수 있었습니다.

## 📌 데이터셋 구성


