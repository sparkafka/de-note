---
layout: post
title: "코딩하면서 ChatGPT에게 얻은 도움"
excerpt: "코딩하면서 ChatGPT에게 도움을 얻었다."
categories: ['Blog']
last_modified_at: 2023-02-06
published: True
---

## 들어가며

코딩 공부를 하거나 회사에서 프로젝트를 수행하다 보면 모르는게 참 많이 나온다. 물론 그럴때는 구글 검색을 가장 많이 사용한다. 그러나 검색할 때 어떻게 검색해야 할지 참 애매할 때가 많다. 이번에도 그런 상황이었는데, ChatGPT가 코딩할 때도 큰 도움이 된다고 들어서 ChatGPT에 도움을 요청해보기로 하였다. 사실 ChatGPT는 나올때부터 가입해서 쓰고 있었는데, 재미로만 몇 번 써봤지, 코딩에 쓸 생각은 하지 않았는데 이번에 써 보았다. 그런데 꽤나 도움이 돼서, 그 경험을 남기고자 포스트를 작성하게 되었다.

## 문제

파이썬에서 원격 서버에 저장할 path를 입력하고 scp를 이용해 원격 서버로 파일을 보내는데, 원격 서버의 path에 지정한 directory가 있는지 확인하고, 없으면 에러를 발생시키는 코드를 작성하고 싶었다. ChatGPT는 영어로 물어볼 때 결과가 더 잘 나오니 다음과 같이 물어봤다. 번역은 갓구글번역기를 이용하였다.   

    How to use ubuntu scp if the remote server's directory exists and cancel otherwise

그랬더니 결과가 다음과 같았다.   

![chatgpt_first](/de-note/assets/images/16th/chatgpt_first.png)

shell script 파일 코드가 나왔다 그래서 파이썬 코드로 알려달라고 물어보았다.

    How to use ubuntu scp if the remote server's directory exists and cancel otherwise in python

그랬더니 이번에는 결과가 다음과 같았다.

![chatgpt_second](/de-note/assets/images/16th/chatgpt_second.png)

파이썬의 subprocess를 이용하는 방법이 출력되었다. 이것도 좋은 방법이지만, 나는 다른 라이브러리를 이용한 방법이 필요했기 때문에 다시 검색하였다.

    How to use ubuntu scp if the remote server's directory exists and cancel otherwise in python scp library

그랬더니 다음과 같은 결과를 얻었다.

![chatgpt_third](/de-note/assets/images/16th/chatgpt_third.png)

이번에는 정확히 내가 원하는, paramiko 라이브러리를 이용하는 결과를 얻었다. 정말 큰 도움이 되었다.

## 마치며

ChatGPT의 대단함을 느낄 수 있는 경험이었다. 확실히 잘 쓰면 프로젝트를 진행하는 데 큰 도움이 될 것 같은 도구인 것 같다. 한 편으론 실력이 떨어지는 개발자들은 금방 도태될 수 있다는 생각도 들었다. 개발에 더 열심히 정진해야 할 것이다.